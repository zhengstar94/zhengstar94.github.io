
# 介绍

Cross-Site Scripting（跨站脚本攻击）简称 XSS，是一种代码注入攻击。攻击者通过在目标网站上注入恶意脚本，使之在用户的浏览器上运行。利用这些恶意脚本，攻击者可获取用户的敏感信息如 Cookie、SessionID 等，进而危害数据安全。

为了和 CSS 区分，这里把攻击的第一个字母改成了 X，于是叫做 XSS。

XSS 的本质是：恶意代码未经过滤，与网站正常的代码混在一起；浏览器无法分辨哪些脚本是可信的，导致恶意脚本被执行。

# 解决

## 1. 导入`hutool-all`包

```xml
<!--    数据转义，防止xss攻击-->
        <dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>5.7.2</version>
        </dependency>
```

## 2. 配置XssHttpServletRequestWrapper

```java
/**
 * @description 对request请求的数据进行转义，防止xss攻击
 * home.php?mod=space&uid=365491 2021/7/4 10:05 上午
 */
public class XssHttpServletRequestWrapper extends HttpServletRequestWrapper {
    public XssHttpServletRequestWrapper(HttpServletRequest request) {
        super(request);
    }

    /**
     * 重写getParameter方法，用HtmlUtil转义后再返回
     */
    @Override
    public String getParameter(String name) {
        String value= super.getParameter(name);
        if(!StrUtil.hasEmpty(value)){
            value=HtmlUtil.filter(value);
        }
        return value;
    }

    /**
     * 重写getParameterValues方法，
     * 遍历每一个值，用HtmlUtil转义后再返回
     */
    @Override
    public String[] getParameterValues(String name) {
        String[] values= super.getParameterValues(name);
        if(values!=null){
            for (int i=0;i<values.length;i++){
                String value=values[i];
                if(!StrUtil.hasEmpty(value)){
                    value=HtmlUtil.filter(value);
                }
                values[i]=value;
            }
        }
        return values;
    }

    /**
     * 重写getParameterMap方法，
     * 拿到所有的k-v键值对，用LinkedHashMap接收，
     * key不变，value用HtmlUtil转义后再返回
     */
    @Override
    public Map<String, String[]> getParameterMap() {
        Map<String, String[]> parameters = super.getParameterMap();
        LinkedHashMap<String, String[]> map=new LinkedHashMap();
        if(parameters!=null){
            for (String key:parameters.keySet()){
                String[] values=parameters.get(key);
                for (int i = 0; i < values.length; i++) {
                    String value = values[i];
                    if (!StrUtil.hasEmpty(value)) {
                        value = HtmlUtil.filter(value);
                    }
                    values[i] = value;
                }
                map.put(key,values);
            }
        }
        return map;
    }

    /**
     * 重写getHeader方法，用HtmlUtil转义后再返回
     */
    @Override
    public String getHeader(String name) {
        String value= super.getHeader(name);
        if (!StrUtil.hasEmpty(value)) {
            value = HtmlUtil.filter(value);
        }
        return value;
    }

    @Override
    public ServletInputStream getInputStream() throws IOException {
        /**
         * 拿到数据流，通过StringBuffer拼接，
         * 读取到line上，用StringBuffer是因为会有多个线程同时请求，要保证线程的安全
         */
        InputStream in= super.getInputStream();
        InputStreamReader reader=new InputStreamReader(in, Charset.forName("UTF-8"));
        BufferedReader buffer=new BufferedReader(reader);
        StringBuffer body=new StringBuffer();
        String line=buffer.readLine();
        while(line!=null){
            body.append(line);
            line=buffer.readLine();
        }
        buffer.close();
        reader.close();
        in.close();

        /**
         * 将拿到的map，转移后存到另一个map中
         */
        Map<String,Object> map=JSONUtil.parseObj(body.toString());
        Map<String,Object> result=new LinkedHashMap<>();
        for(String key:map.keySet()){
            Object val=map.get(key);
            if(val instanceof String){
                if(!StrUtil.hasEmpty(val.toString())){
                    result.put(key,HtmlUtil.filter(val.toString()));
                }
            }else {
                result.put(key,val);
            }
        }
        String json=JSONUtil.toJsonStr(result);
        ByteArrayInputStream bain=new ByteArrayInputStream(json.getBytes());
        //匿名内部类，只需要重写read方法，把转义后的值，创建成ServletInputStream对象
        return new ServletInputStream() {
            @Override
            public int read() throws IOException {
                return bain.read();
            }

            @Override
            public boolean isFinished() {
                return false;
            }

            @Override
            public boolean isReady() {
                return false;
            }

            @Override
            public void setReadListener(ReadListener readListener) {

            }
        };
    }
}
```

## 3. 配置XssFilter

```java

/**
 * 拦截所有的请求，对所有请求转义
 */
@WebFilter(urlPatterns = "/*")
public class XssFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    /**
     * 将获取到的数据，进行转义后再放行
     * home.php?mod=space&uid=952169 servletRequest 请求
     * @param servletResponse 响应
     * @param filterChain 拦截链
     * @throws IOException
     * @throws ServletException
     */
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest request= (HttpServletRequest) servletRequest;
        XssHttpServletRequestWrapper wrapper=new XssHttpServletRequestWrapper(request);
        filterChain.doFilter(wrapper,servletResponse);
    }

    @Override
    public void destroy() {

    }
}
```



