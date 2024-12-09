---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "468. Validate IP Address"
date: "2024-12-09"
tags: Medium
categories:
  - "LeetCode String"
---

- Given a string `queryIP`, return `"IPv4"` if IP is a valid IPv4 address, `"IPv6"` if IP is a valid IPv6 address or `"Neither"` if IP is not a correct IP of any type.

- **A valid IPv4** address is an IP in the form `"x1.x2.x3.x4"` where `0 <= xi <= 255` and `xi` **cannot contain** leading zeros. For example, `"192.168.1.1"` and `"192.168.1.0"` are valid IPv4 addresses while `"192.168.01.1"`, `"192.168.1.00"`, and `"192.168@1.1"` are invalid IPv4 addresses.

- **A valid IPv6** address is an IP in the form `"x1:x2:x3:x4:x5:x6:x7:x8"` where:

  - `1 <= xi.length <= 4`
  - `xi` is a **hexadecimal string** which may contain digits, lowercase English letter (`'a'` to `'f'`) and upper-case English letters (`'A'` to `'F'`).
  - Leading zeros are allowed in `xi`.

- For example, "`2001:0db8:85a3:0000:0000:8a2e:0370:7334"` and "`2001:db8:85a3:0:0:8A2E:0370:7334"` are valid IPv6 addresses, while "`2001:0db8:85a3::8A2E:037j:7334"` and "`02001:0db8:85a3:0000:0000:8a2e:0370:7334"` are invalid IPv6 addresses.



**Example 1**

```
Input: queryIP = "172.16.254.1"
Output: "IPv4"
Explanation: This is a valid IPv4 address, return "IPv4".
```

**Example 2**

```
Input: queryIP = "2001:0db8:85a3:0:0:8A2E:0370:7334"
Output: "IPv6"
Explanation: This is a valid IPv6 address, return "IPv6".
```

**Example 3**

```
Input: queryIP = "256.256.256.256"
Output: "Neither"
Explanation: This is neither a IPv4 address nor a IPv6 address.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.String;

import java.util.Objects;

/**
 * @author zhengxingxing
 * @date 2024/12/09
 */
public class ValidateIPAddress {
    // Constants for IP address types
    public static final String IPV4 = "IPv4";
    public static final String IPV6 = "IPv6";
    public static final String NEITHER = "Neither";


    public static String validIPAddress(String queryIP) {
        // Check if it's an IPv4 address (contains dot)
        if(queryIP.contains(".")){
            return handleIPV4(queryIP);
        }
        // Check if it's an IPv6 address (contains colon)
        else if (queryIP.contains(":")){
            return handleIPV6(queryIP);
        }
        // If neither dot nor colon, it's an invalid address
        else{
            return NEITHER;
        }
    }

    /**
     * Validate IPv4 address
     *
     * @param queryIP the IPv4 address to validate
     * @return "IPv4" if valid, "Neither" otherwise
     */
    private static String handleIPV4(String queryIP){
        // Split the IP address into parts
        String[] ipParts = queryIP.split("\\.");

        // Check basic IPv4 structure
        if(ipParts.length != 4 || queryIP.startsWith(".") || queryIP.endsWith(".")){
            return NEITHER;
        }

        // Validate each part of the IPv4 address
        for (String ipPart: ipParts) {
            // Check for invalid leading zeros
            if(ipPart.startsWith("0") && !Objects.equals(ipPart, "0")){
                return NEITHER;
            }

            try {
                // Convert part to integer and check range
                int ipPartInt = Integer.parseInt(ipPart);
                if(ipPartInt < 0 || ipPartInt > 255){
                    return NEITHER;
                }
            }catch (Exception e){
                // If conversion fails, it's invalid
                return NEITHER;
            }
        }
        return IPV4;
    }

    /**
     * Validate IPv6 address
     *
     * @param queryIP the IPv6 address to validate
     * @return "IPv6" if valid, "Neither" otherwise
     */
    private static String handleIPV6(String queryIP) {
        // Split the IP address into parts
        String[] ipParts = queryIP.split(":");

        // Check basic IPv6 structure
        if(ipParts.length != 8 || queryIP.startsWith(":") || queryIP.endsWith(":")){
            return NEITHER;
        }

        // Validate each part of the IPv6 address
        for (String ipPart: ipParts) {
            // Check part length (1-4 characters)
            if (ipPart.length() < 1 || ipPart.length() > 4){
                return NEITHER;
            }

            // Validate each character
            for (char c: ipPart.toCharArray()) {
                // Only allow 0-9, a-f, A-F
                if ((c < '0' || c > '9') && (c < 'a' || c > 'f') && (c < 'A' || c > 'F')){
                    return NEITHER;
                }
            }
        }
        return IPV6;
    }
    
    public static void main(String[] args) {

        // IPv4 test cases
        String[] ipv4TestCases = {
                "172.16.254.1",       // Valid IPv4
                "0.0.0.0",            // Special valid IPv4
                "255.255.255.255",    // Maximum IPv4
                "256.256.256.256",    // Invalid IPv4 - Out of range
                "1.1.1.1.",           // Invalid IPv4 - Ends with dot
                "01.01.01.01",        // Invalid IPv4 - Leading zeros
                "123.045.067.089"     // Invalid IPv4 - Leading zeros
        };

        // IPv6 test cases
        String[] ipv6TestCases = {
                "2001:0db8:85a3:0:0:8A2E:0370:7334", // Valid IPv6
                "2001:db8:85a3:01:0:8A2E:0370:7334", // Valid IPv6 - Single character part
                "2001:0db8:85a3:0:0:8A2E:0370:7334", // Valid IPv6
                "2001:0db8:85a3:0000:0000:8A2E:0370:7334", // Valid IPv6
                "::1",                // Invalid IPv6 - Insufficient segments
                "1:2:3:4:5:6:7:8:9",  // Invalid IPv6 - Too many segments
                "2001:0db8:85a3:0:0:8A2E:0370:g334" // Invalid IPv6 - Invalid character
        };

        // Test IPv4 cases
        System.out.println("IPv4 Test Results:");
        for (String testCase : ipv4TestCases) {
            System.out.println(testCase + " : " + validIPAddress(testCase));
        }

        // Test IPv6 cases
        System.out.println("\nIPv6 Test Results:");
        for (String testCase : ipv6TestCases) {
            System.out.println(testCase + " : " + validIPAddress(testCase));
        }
    }
}
```





