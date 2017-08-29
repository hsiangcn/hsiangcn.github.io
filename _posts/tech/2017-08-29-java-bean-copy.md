---  
layout: post  
title: 利用反射处理-使用BeanUtils.copyProperties复制对象时间问题   
category: 技术  
tags: java  
keywords: java  
---  

　　BeanUtils.copyProperties() 方法在处理数据类型为日期的属性的值的时候，如果值为空的情况下，判断不出源数据的类型，所以会抛出异常。<br>
　　建议使用[ConvertUtils.register](http://hsiang.org/2017/08/29/convertUtils_register.html)为日期类型指定一个为空的情况下使用的默认值，当BeanUtils遇到目标类型为日期格式，并且源数据值为空的情况下，会默认取设置的这个默认值<br>

## 1、 简介
　　BeanUtils提供对Java反射和自省API的包装。其主要目的是利用反射机制对JavaBean的属性进行处理。
我们知道，一个JavaBean通常包含了大量的属性，很多情况下，对JavaBean的处理导致大量get/set代码堆积，
增加了代码长度和阅读代码的难度。

## 2、 缺点
　　BeanUtils的成本惊人地昂贵，效率要远远的超过超过取数 据、将其复制到对应的 value对象。<br>
　　**该方法的效率较低，在使用的时候请慎重考虑。**
## 3、 源码
　　话不多说，直接上源码

### 3.1、 时间格式化工具类
    package com.util;
    
    import java.sql.Timestamp;
    import java.text.ParseException;
    import java.text.SimpleDateFormat;
    import java.util.Date;
    
    public class DateUtils {
    
        public static final String YYYYMMDD = "yyyy-MM-dd";
    
        public static final String YYYYMMDDHHMMSS = "yyyy-MM-dd HH:mm:ss";
    
        /**
         *  String 转 Date
         *
         *
         * @param time
         * @param format
         * @return
         */
        public static Date stringToDate(String time, String format) {
            Date date = null;
            if (time != null) {
                try {
                    date = (new SimpleDateFormat(format)).parse(time);
                } catch (ParseException e) {
                    e.printStackTrace();
                }
            }
    
            return date;
        }
    
        /**
         *  String 转 timestamp
         *
         *
         * @param time
         * @param format
         * @return
         */
        public static Timestamp stringToTimestamp(String time, String format) {
            // Timestamp.valueOf(time);
            // 该方法time格式必须为yyyy-MM-dd HH:mm:ss[:SSS]
            Timestamp timestamp = null;
            SimpleDateFormat sdf = new SimpleDateFormat(format);
            try {
                Date date = sdf.parse(time);
                timestamp = new Timestamp(date.getTime());
            } catch (ParseException e) {
                e.printStackTrace();
            }
            return timestamp;
        }
    
        /**
         *  Date 转 String
         *
         *
         * @param time
         * @param format
         * @return
         */
        public static String dateToString(Date time, String format) {
            String date = null;
            if (time != null) {
                date = (new SimpleDateFormat(format)).format(time);
            }
    
            return date;
        }
    
        /**
         *  Date 转 timestamp
         *
         *
         * @param time
         * @return
         */
        public static Timestamp dateToTimestamp(Date time) {
            // 父类不能直接向子类转化，可借助中间的String
            // 该方法转出来的Timestamp格式为yyyy-MM-dd HH:mm:ss:SSS
            Timestamp timestamp = new Timestamp(time.getTime());
            return timestamp;
        }
    
    
        /**
         *  timestamp 转 String
         *
         *
         * @param time
         * @param format
         * @return
         */
        public static String timestampToString(Timestamp time, String format) {
            String dateStr = null;
            if (time != null) {
                dateStr = (new SimpleDateFormat(format)).format(time);
            }
    
            return dateStr;
        }
    
        /**
         *  timestamp 转 Date
         *
         *
         * @param time
         * @param format
         * @return
         */
        public static Date timestampToDate(Timestamp time, String format) {
            Date date = time;
            if (format != null) {
                try {
                    SimpleDateFormat sdf = new SimpleDateFormat(format);
                    date = sdf.parse(sdf.format(date));
                } catch (ParseException e) {
                    e.printStackTrace();
                }
            }
            return date;
        }
    
    
    }  
    
### 3.2、 首字母大写转换方法
    /**
     *  首字母大写转换
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
    
　　[详情请见首字母大小写转换](2017-08-29-capitalize_initial_letters.md)

### 3.3、对象复制
　　**该方法的效率较低，在使用的时候请慎重考虑。**
    
    /**
     *  利用反射处理使用BeanUtils.copyProperties复制对象时时间无法复制问题
     *
     * @param source 被复制的对象
     * @param cla  需要赋值的对象类型
     * @param ignoreProperties 需要过滤使用反射进行赋值的字段
     * @return
     * @throws Exception
     */
    private static Object transformToDTO(Object source, Class cla,  String... ignoreProperties) throws IllegalAccessException, InstantiationException {

        Object target = cla.newInstance();
        BeanUtils.copyProperties(source, target, ignoreProperties);
        List<String> ignoreList = (ignoreProperties != null ? Arrays.asList(ignoreProperties) : null);

        if (ignoreList == null || ignoreList.isEmpty()) {
            return null;
        }


        Class cls = source.getClass();
        Field[] fields = cls.getDeclaredFields();
        for (Field field : fields) {
            String name = field.getName();
            if (ignoreList.contains(name)) {
                try {
                    // 首字母转大写
                    String captureName = captureName(name);
                    Object obj =  cls.getMethod("get" + captureName).invoke(source);
                    Class tarClass = target.getClass();
                    Class clazz = tarClass.getDeclaredField(name).getType();
                    Method method = tarClass.getMethod("set" + captureName, clazz);
                    Object time = null;
                    if (obj instanceof Timestamp) {
                        if (clazz.newInstance() instanceof String) {
                            // Timestamp 转 String
                            time = DateUtils.timestampToString((Timestamp)obj, DateUtils.YYYYMMDDHHMMSS);
                        } else if (clazz.newInstance() instanceof Date) {
                            // Timestamp 转 Date
                            time = DateUtils.timestampToDate((Timestamp)obj, DateUtils.YYYYMMDDHHMMSS);
                        } else if (clazz.newInstance() instanceof Timestamp) {
                            // Timestamp 转 Timestamp
                            time = obj;
                        }

                    } else if (obj instanceof Date) {
                        if (clazz.newInstance() instanceof String) {
                            // Date 转 String
                            time = DateUtils.dateToString((Date)obj, DateUtils.YYYYMMDDHHMMSS);
                        } else if (clazz.newInstance() instanceof Timestamp) {
                            // Date 转 Timestamp
                            time = DateUtils.dateToTimestamp((Date)obj);
                        } else if (clazz.newInstance() instanceof Date) {
                            // Date 转 Date
                            time = obj;
                        }
                    } else if (obj instanceof String) {
                        if (clazz.newInstance() instanceof String) {
                            // String 转 String
                            time = obj;
                        } else if (clazz.newInstance() instanceof Timestamp) {
                            // String 转 Timestamp
                            time = DateUtils.stringToTimestamp((String)obj, DateUtils.YYYYMMDDHHMMSS);
                        } else if (clazz.newInstance() instanceof Date) {
                            // String 转 Date
                            time = DateUtils.stringToDate((String)obj, DateUtils.YYYYMMDDHHMMSS);
                        }
                    }
                    method.invoke(target, time);
                } catch (Exception ex) {
                    logger.info(" copy object falied ", ex);
                }

            }
        }

        return target;
    }