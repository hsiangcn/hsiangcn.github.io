---  
layout: post  
title: 字符串的首字母大小写转换  
category: 技术  
tags: java、ASCII  
keywords: java、ASCII  
---  

将首字母转大写比较常用的有两种方式，个人比较推荐第二种。

## 1. 使用截取方式
    /**
     *  首字母转小写
     * @param name
     * @return
     */
    public static String toLowerCaseFirstOne(String name){
        if(Character.isLowerCase(name.charAt(0))) {
            return name;
        } else {
            return (new StringBuilder()).append(Character.toLowerCase(name.charAt(0))).append(s.substring(1)).toString();
        }
    }

    /**
     *  首字母转大写
     * @param name
     * @return
     */
    public static String toUpperCaseFirstOne(String name){
        if(Character.isUpperCase(name.charAt(0))) {
            return name;
        } else {
            return (new StringBuilder()).append(Character.toUpperCase(name.charAt(0))).append(s.substring(1)).toString();
        }
    }

## 2. 使用截取方式
    /**
     *   字符串的首字母大写转换  
     * @param name
     * @return
     */
    public static String captureName(String name) {
        if (name == null || "".equals(name)) {
            return name;
        }

        char[] cs = name.toCharArray();
        // 根据ASCII表，小写字母在97到122之间
        if (97 <= cs[0] && cs[0] <= 122 ) {
            cs[0]-=32;
            return String.valueOf(cs);
        }
        return name;
     }
     
    /**
     *   字符串的首字母小写转换  
     * @param name
     * @return
     */
    public static String captureName(String name) {
         if (name == null || "".equals(name)) {
             return name;
         }
         char[] cs = name.toCharArray();
         // 根据ASCII表，大写字母在65到90之间
         if (65 <= cs[0] && cs[0] <= 90 ) {
             cs[0]+=32;
             return String.valueOf(cs);
         }
         return name;
    }