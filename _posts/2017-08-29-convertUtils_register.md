---  
layout: post  
title: BeanUtils.copyProperties时日期的拷贝   
categories: JAVA  
description: BeanUtils.copyProperties时日期的拷贝  
keywords: java  
---  

　　BeanUtils.copyProperties() 方法在处理数据类型为日期的属性的值的时候，如果值为空的情况下，判断不出源数据的类型，所以会抛出异常。<br>
　　可以使用ConvertUtils.register为日期类型指定一个为空的情况下使用的默认值，当BeanUtils遇到目标类型为日期格式，并且源数据值为空的情况下，会默认取设置的这个默认值<br>
　　首先定义一个Converter的Date实现类

    import java.util.Date;
    
    import org.apache.commons.beanutils.ConversionException;
    import org.apache.commons.beanutils.Converter;
    
    import cn.sh.cares.commons.excel.util.ExcelUtils;
    
    public class DateConverter implements Converter {
    
        public Object convert(Class type, Object value) {
        if(value == null) {
            return null;
        }
        
        if(value instanceof Date) {
            return value;
        }
        
        if(value instanceof Long) {
            Long longValue = (Long) value;
            return new Date(longValue.longValue());
        }
        
            try {
                return ExcelUtils.SIMPLE_DATE_FORMATER.parse(value.toString());
            } catch (Exception e) {
                throw new ConversionException(e);
            }
        }
    }
    
　　这个主要完成由String或者Long向Date型拷贝时如何转换，这样我们在转换之前只要在ConvertUtils中注册这个Converter就可以了，比如下面A和B类<br>
    
    public class A {
        private String start;
        private Date end;
        public Date getEnd() {
            return end;
        }
        public void setEnd(Date end) {
            this.end = end;
        }
        public String getStart() {
            return start;
        }
        public void setStart(String start) {
            this.start = start;
        }
        public String toString() {
        return "start = " + this.start + ", end = " + end;
        }
    }
    public class B {
        private Date start;
        private String end;
        public String getEnd() {
            return end;
        }
        public void setEnd(String end) {
            this.end = end;
        }
        public Date getStart() {
            return start;
        }
        public void setStart(Date start) {
            this.start = start;
        }
        
        public String toString() {
        return "start = " + this.start + ", end = " + end;
        }
    }
    
转换测试程序如下：
    
    import java.util.Date;
    
    import org.apache.commons.beanutils.BeanUtils;
    import org.apache.commons.beanutils.ConvertUtils;
    
    public class MyTest {
    
        public static void main(String[] args) throws Exception {
        ConvertUtils.register(new DateConverter(), Date.class);
        //替换原来的StringConverter
        ConvertUtils.register(new MyStringConverter(), String.class);
        A a = new A();
        a.setEnd(new Date());
        a.setStart("2007-05-06");
        B b = new B();
        BeanUtils.copyProperties(b, a);
        System.out.println(b);
        }
    }
    
这样子，输出结果为：
start = Sun May 06 00:00:00 CST 2007, end = 2007-11-28

转自:[http://blog.csdn.net/wangyuxuan_java/article/details/8589730](http://blog.csdn.net/wangyuxuan_java/article/details/8589730)