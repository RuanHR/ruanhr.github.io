---
layout: post
title: java时间工具类
date: 2017-09-21
categories: blog
tags: [java]
description: 时间工具类
---

介绍一个java实现的时间工具类
### TimeUtils
时间工具类
- millis2String : 将时间戳转为时间字符串
- string2Millis : 将时间字符串转为时间戳
- string2Date : 将时间字符串转为Date类型
- date2String : 将Date类型转为时间字符串
- date2Millis : 将Date类型转为时间戳
- millis2Date : 将时间戳转为Date类型
- dateReckon : 日期+-天数
- getNowTimeMills : 获取当前毫秒时间戳
- getNowTimeString : 获取当前时间字符串
- getNowTimeDate : 获取当前Date
- getWeek, getWeekIndex : 获取星期
- getWeekOfMonth : 获取月份中的第几周
- getWeekOfYear : 获取年份中的第几周
- parseMillisecone : 时间差计算 return string 0天0时11分55秒
- getDifference ：时间差计算 return int
- getChineseZodiac : 获取生肖
- getZodiac : 获取星座
注意：SimpleDateFormat不是线程安全的，线程安全需用{@code ThreadLocal<SimpleDateFormat>}
```java
package com.pactera.base.utils;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;
import java.util.GregorianCalendar;
import java.util.Locale;

/**
 * @ClassName: TimeUtils
 * @date 2017年9月18日 下午12:32:16
 * @author ToniR
 * @Description: 时间相关操作工具类
 */
public class TimeUtils {

    static final SimpleDateFormat DEFAULT_SDF = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss", Locale.getDefault());

    static final SimpleDateFormat DEFAULT_SIMPLE_SDF = new SimpleDateFormat("yyyy-MM-dd", Locale.getDefault());

    static final String DEFAULT_TIME = "yyyy-MM-dd HH:mm:ss";

    static final String DEFAULT_SIMPLE_TIME = "yyyy-MM-dd";

    static final long DAY = 1000 * 60 * 60 * 24;

    static final long HOUR = 1000 * 60 * 60;

    static final long MINUTE = 1000 * 60;

    static final long SECOND = 1000;

    /**
     * 将时间戳转为时间字符串 格式为yyyy-MM-dd HH:mm:ss
     *
     * @param millis
     *            毫秒时间戳
     * @return 时间字符串
     */
    public static String millis2String(long millis) {
        return DEFAULT_SDF.format(new Date(millis));
    }

    /**
     * 将时间戳转为时间字符串 格式为pattern
     *
     * @param millis
     *            毫秒时间戳
     * @param pattern
     *            时间格式
     * @return 时间字符串
     */
    public static String millis2String(long millis, String pattern) {
        return new SimpleDateFormat(pattern, Locale.getDefault()).format(new Date(millis));
    }

    /**
     * 将时间字符串转为时间戳 time格式为yyyy-MM-dd HH:mm:ss
     *
     * @param time
     *            时间字符串
     * @param pattern
     *            时间格式
     * @return 毫秒时间戳
     */
    public static long string2Millis(String time) {
        return string2Millis(time, DEFAULT_TIME);
    }

    /**
     * 将时间字符串转为时间戳 time格式为pattern
     *
     * @param time
     *            时间字符串
     * @param pattern
     *            时间格式
     * @return 毫秒时间戳
     */
    public static long string2Millis(String time, String pattern) {
        try {
            return new SimpleDateFormat(pattern, Locale.getDefault()).parse(time).getTime();
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return -1;
    }

    /**
     * 将时间字符串转为Date类型 time格式为yyyy-MM-dd HH:mm:ss
     *
     * @param time
     *            时间字符串
     * @return Date类型
     */
    public static Date string2Date(String time) {
        return string2Date(time, DEFAULT_TIME);
    }

    /**
     * 将时间字符串转为Date类型
     *
     * time格式为pattern
     *
     * @param time
     *            时间字符串
     * @param pattern
     *            时间格式
     * @return Date类型
     */
    public static Date string2Date(String time, String pattern) {
        return new Date(string2Millis(time, pattern));
    }

    /**
     * 将Date类型转为时间字符串 格式为yyyy-MM-dd HH:mm:ss
     *
     * @param date
     *            Date类型时间
     * @return 时间字符串
     */
    public static String date2String(Date date) {
        return date2String(date, DEFAULT_TIME);
    }

    /**
     * 将Date类型转为时间字符串 格式为pattern
     *
     * @param date
     *            Date类型时间
     * @param pattern
     *            时间格式
     * @return 时间字符串
     */
    public static String date2String(Date date, String pattern) {
        return new SimpleDateFormat(pattern, Locale.getDefault()).format(date);
    }

    /**
     * 将Date类型转为时间戳
     *
     * @param date
     *            Date类型时间
     * @return 毫秒时间戳
     */
    public static long date2Millis(Date date) {
        return date.getTime();
    }

    /**
     * 将时间戳转为Date类型
     *
     * @param millis
     *            毫秒时间戳
     * @return Date类型时间
     */
    public static Date millis2Date(long millis) {
        return new Date(millis);
    }

    /**
     * 日期+-天数
     *
     * @param date
     * @param num
     *            增加的天数，整数为+，负数-
     * @return Date
     */
    public static Date dateReckon(Date date, Integer num) {
        Calendar calendar = new GregorianCalendar();
        calendar.setTime(date);
        calendar.add(Calendar.DATE, num);// 把日期往后增加一天.整数往后推,负数往前移动
        date = calendar.getTime(); // 这个时间就是日期往后推一天的结果
        return date;
    }

    /**
     * 日期+-天数
     *
     * @param date
     * @param num
     *            增加的天数，整数为+，负数-
     * @return Date
     */
    public static Date dateReckon(long millis, Integer num) {
        Calendar calendar = new GregorianCalendar();
        calendar.setTime(millis2Date(millis));
        calendar.add(Calendar.DATE, num);// 把日期往后增加一天.整数往后推,负数往前移动
        Date date = calendar.getTime(); // 这个时间就是日期往后推一天的结果
        return date;
    }

    /**
     * 日期+-天数
     *
     * @param date
     * @param num
     *            增加的天数，整数为+，负数-
     * @return Date
     */
    public static Date dateReckon(String time, Integer num) {
        Calendar calendar = new GregorianCalendar();
        calendar.setTime(string2Date(time));
        calendar.add(Calendar.DATE, num);// 把日期往后增加一天.整数往后推,负数往前移动
        Date date = calendar.getTime(); // 这个时间就是日期往后推一天的结果
        return date;
    }

    /**
     * 获取当前毫秒时间戳
     *
     * @return 毫秒时间戳
     */
    public static long getNowTimeMills() {
        return System.currentTimeMillis();
    }

    /**
     * 获取当前时间字符串 格式为yyyy-MM-dd HH:mm:ss
     *
     * @return 时间字符串
     */
    public static String getNowTimeString() {
        return millis2String(System.currentTimeMillis(), DEFAULT_TIME);
    }

    /**
     * 获取当前Date
     *
     * @return Date类型时间
     */
    public static Date getNowTimeDate() {
        return new Date();
    }

    /**
     * 获取星期 time格式为yyyy-MM-dd HH:mm:ss
     *
     * @param time
     *            时间字符串
     * @return 星期
     */
    public static String getWeek(String time) {
        return getWeek(string2Date(time, DEFAULT_TIME));
    }

    /**
     * 获取星期 time格式为pattern
     *
     * @param time
     *            时间字符串
     * @param pattern
     *            时间格式
     * @return 星期
     */
    public static String getWeek(String time, String pattern) {
        return getWeek(string2Date(time, pattern));
    }

    /**
     * 获取星期
     *
     * @param date
     *            Date类型时间
     * @return 星期
     */
    public static String getWeek(Date date) {
        return new SimpleDateFormat("EEEE", Locale.getDefault()).format(date);
    }

    /**
     * 获取星期
     *
     * @param millis
     *            毫秒时间戳
     * @return 星期
     */
    public static String getWeek(long millis) {
        return getWeek(new Date(millis));
    }

    /**
     * 获取星期 注意：周日的Index才是1，周六为7 time格式为yyyy-MM-dd HH:mm:ss
     *
     * @param time
     *            时间字符串
     * @return 1...5
     */
    public static int getWeekIndex(String time) {
        return getWeekIndex(string2Date(time, DEFAULT_TIME));
    }

    /**
     * 获取星期 注意：周日的Index才是1，周六为7 time格式为pattern
     *
     * @param time
     *            时间字符串
     * @param pattern
     *            时间格式
     * @return 1...7
     */
    public static int getWeekIndex(String time, String pattern) {
        return getWeekIndex(string2Date(time, pattern));
    }

    /**
     * 获取星期 注意：周日的Index才是1，周六为7
     *
     * @param date
     *            Date类型时间
     * @return 1...7
     */
    public static int getWeekIndex(Date date) {
        Calendar cal = Calendar.getInstance();
        cal.setTime(date);
        return cal.get(Calendar.DAY_OF_WEEK);
    }

    /**
     * 获取星期 注意：周日的Index才是1，周六为7
     *
     * @param millis
     *            毫秒时间戳
     * @return 1...7
     */
    public static int getWeekIndex(long millis) {
        return getWeekIndex(millis2Date(millis));
    }

    /**
     * 获取月份中的第几周
     * <p>
     * 注意：国外周日才是新的一周的开始
     * </p>
     * <p>
     * time格式为yyyy-MM-dd HH:mm:ss
     * </p>
     *
     * @param time
     *            时间字符串
     * @return 1...5
     */
    public static int getWeekOfMonth(String time) {
        return getWeekOfMonth(string2Date(time, DEFAULT_TIME));
    }

    /**
     * 获取月份中的第几周 注意：国外周日才是新的一周的开始 time格式为pattern
     *
     * @param time
     *            时间字符串
     * @param pattern
     *            时间格式
     * @return 1...5
     */
    public static int getWeekOfMonth(String time, String pattern) {
        return getWeekOfMonth(string2Date(time, pattern));
    }

    /**
     * 获取月份中的第几周 注意：国外周日才是新的一周的开始
     *
     * @param date
     *            Date类型时间
     * @return 1...5
     */
    public static int getWeekOfMonth(Date date) {
        Calendar cal = Calendar.getInstance();
        cal.setTime(date);
        return cal.get(Calendar.WEEK_OF_MONTH);
    }

    /**
     * 获取月份中的第几周 注意：国外周日才是新的一周的开始
     *
     * @param millis
     *            毫秒时间戳
     * @return 1...5
     */
    public static int getWeekOfMonth(long millis) {
        return getWeekOfMonth(millis2Date(millis));
    }

    /**
     * 获取年份中的第几周 注意：国外周日才是新的一周的开始 time格式为yyyy-MM-dd HH:mm:ss
     *
     * @param time
     *            时间字符串
     * @return 1...54
     */
    public static int getWeekOfYear(String time) {
        return getWeekOfYear(string2Date(time, DEFAULT_TIME));
    }

    /**
     * 获取年份中的第几周 注意：国外周日才是新的一周的开始 time格式为pattern
     *
     * @param time
     *            时间字符串
     * @param pattern
     *            时间格式
     * @return 1...54
     */
    public static int getWeekOfYear(String time, String pattern) {
        return getWeekOfYear(string2Date(time, pattern));
    }

    /**
     * 获取年份中的第几周 注意：国外周日才是新的一周的开始
     *
     * @param date
     *            Date类型时间
     * @return 1...54
     */
    public static int getWeekOfYear(Date date) {
        Calendar cal = Calendar.getInstance();
        cal.setTime(date);
        return cal.get(Calendar.WEEK_OF_YEAR);
    }

    /**
     * 获取年份中的第几周 注意：国外周日才是新的一周的开始
     *
     * @param millis
     *            毫秒时间戳
     * @return 1...54
     */
    public static int getWeekOfYear(long millis) {
        return getWeekOfYear(millis2Date(millis));
    }

    private static final String[] CHINESE_ZODIAC = { "猴", "鸡", "狗", "猪", "鼠", "牛", "虎", "兔", "龙", "蛇", "马", "羊" };

    /**
     * 获取生肖 time格式为yyyy-MM-dd HH:mm:ss
     *
     * @param time
     *            时间字符串
     * @return 生肖
     */
    public static String getChineseZodiac(String time) {
        return getChineseZodiac(string2Date(time, DEFAULT_TIME));
    }

    /**
     * 获取生肖 time格式为pattern
     *
     * @param time
     *            时间字符串
     * @param pattern
     *            时间格式
     * @return 生肖
     */
    public static String getChineseZodiac(String time, String pattern) {
        return getChineseZodiac(string2Date(time, pattern));
    }

    /**
     * 获取生肖
     *
     * @param date
     *            Date类型时间
     * @return 生肖
     */
    public static String getChineseZodiac(Date date) {
        Calendar cal = Calendar.getInstance();
        cal.setTime(date);
        return CHINESE_ZODIAC[cal.get(Calendar.YEAR) % 12];
    }

    /**
     * 获取生肖
     *
     * @param millis
     *            毫秒时间戳
     * @return 生肖
     */
    public static String getChineseZodiac(long millis) {
        return getChineseZodiac(millis2Date(millis));
    }

    /**
     * 获取生肖
     *
     * @param year
     *            年
     * @return 生肖
     */
    public static String getChineseZodiac(int year) {
        return CHINESE_ZODIAC[year % 12];
    }

    private static final String[] ZODIAC = { "水瓶座", "双鱼座", "白羊座", "金牛座", "双子座", "巨蟹座", "狮子座", "处女座", "天秤座", "天蝎座",
            "射手座", "魔羯座" };
    private static final int[] ZODIAC_FLAGS = { 20, 19, 21, 21, 21, 22, 23, 23, 23, 24, 23, 22 };

    /**
     * 获取星座 time格式为yyyy-MM-dd HH:mm:ss
     *
     * @param time
     *            时间字符串
     * @return 生肖
     */
    public static String getZodiac(String time) {
        return getZodiac(string2Date(time, DEFAULT_TIME));
    }

    /**
     * 获取星座 time格式为pattern
     *
     * @param time
     *            时间字符串
     * @param pattern
     *            时间格式
     * @return 生肖
     */
    public static String getZodiac(String time, String pattern) {
        return getZodiac(string2Date(time, pattern));
    }

    /**
     * 获取星座
     *
     * @param date
     *            Date类型时间
     * @return 星座
     */
    public static String getZodiac(Date date) {
        Calendar cal = Calendar.getInstance();
        cal.setTime(date);
        int month = cal.get(Calendar.MONTH) + 1;
        int day = cal.get(Calendar.DAY_OF_MONTH);
        return getZodiac(month, day);
    }

    /**
     * 获取星座
     *
     * @param millis
     *            毫秒时间戳
     * @return 星座
     */
    public static String getZodiac(long millis) {
        return getZodiac(millis2Date(millis));
    }

    /**
     * 获取星座
     *
     * @param month
     *            月
     * @param day
     *            日
     * @return 星座
     */
    public static String getZodiac(int month, int day) {
        return ZODIAC[day >= ZODIAC_FLAGS[month - 1] ? month - 1 : (month + 10) % 12];
    }

    /**
     * 时间差计算
     *
     * @param time
     *            当前时间
     * @return string 0天0时11分55秒
     */
    public static String parseMillisecone(Date time) {

        return parseMillisecone(countTimeDiff(date2Millis(time)));
    }

    /**
     * 时间差计算
     *
     * @param time
     *            当前时间
     * @return string 0天0时11分55秒
     */
    public static String parseMillisecone(String time) {

        return parseMillisecone(countTimeDiff(string2Millis(time)));
    }

    /**
     * 时间差计算
     *
     * @param time
     *            当前时间
     * @return string 0天0时11分55秒
     */
    public static String parseMillisecone(String time, String pattern) {

        return parseMillisecone(countTimeDiff(string2Millis(time, pattern)));
    }

    /**
     * 时间差计算
     *
     * @param millisecond
     * @return string 0天0时11分55秒
     */
    public static String parseMillisecone(long millisecond) {
        String time = null;
        try {
            long yushu_day = millisecond % DAY;
            long yushu_hour = millisecond % (DAY * HOUR);
            long yushu_minute = millisecond % (DAY * HOUR * MINUTE);
            @SuppressWarnings("unused")
            long yushu_second = millisecond % (DAY * HOUR * MINUTE * SECOND);
            if (yushu_day == 0) {
                return (millisecond / DAY) + "天";
            } else {
                if (yushu_hour == 0) {
                    return (millisecond / DAY) + "天" + (yushu_day / HOUR) + "时";
                } else {
                    if (yushu_minute == 0) {
                        return (millisecond / DAY) + "天" + (yushu_day / HOUR) + "时" + (yushu_hour / MINUTE) + "分";
                    } else {
                        return (millisecond / DAY) + "天" + (yushu_day / HOUR) + "时" + (yushu_hour / MINUTE) + "分"
                                + (yushu_minute / SECOND) + "秒";
                    }
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        return time;
    }

    /**
     * 时间差计算
     *
     * @param time
     *            当前时间
     * @return string 0天0时11分55秒
     */
    public static long getDifference(Date time, int f) {

        return getDifference(countTimeDiff(date2Millis(time)), f);
    }

    /**
     * 时间差计算
     *
     * @param time
     *            当前时间
     * @return string 0天0时11分55秒
     */
    public static long getDifference(String time, int f) {

        return getDifference(countTimeDiff(string2Millis(time)), f);
    }

    /**
     * 时间差计算
     *
     * @param time
     *            当前时间
     * @return string 0天0时11分55秒
     */
    public static long getDifference(String time, String pattern, int f) {

        return getDifference(countTimeDiff(string2Millis(time, pattern)), f);
    }

    /**
     * 计算时间与当前时间相差多久
     *
     * @param time
     * @param f
     *            时间差的形式0:秒,1:分种,2:小时,3:天
     * @return 返回int类型长度，多出则+1
     */
    public static long getDifference(long time, int f) {
        long result = 0;
        long yushu_day = time % DAY;
        long yushu_hour = time % (DAY * HOUR);
        long yushu_minute = time % (DAY * HOUR * MINUTE);
        long yushu_second = time % (DAY * HOUR * MINUTE * SECOND);
        try {
            // 日期相减获取日期差X(单位:毫秒)
            long millisecond = System.currentTimeMillis() - time;
            /**
             * Math.abs((int)(millisecond/1000)); 绝对值 1秒 = 1000毫秒
             * millisecond/1000 --> 秒 millisecond/1000*60 - > 分钟
             * millisecond/(1000*60*60) -- > 小时 millisecond/(1000*60*60*24) -->
             * 天
             */
            switch (f) {
            case 0: {
                if (yushu_second != 0) {
                    return (millisecond / SECOND) + 1;
                }
                return (millisecond / SECOND);
            }
            case 1: {
                if (yushu_minute != 0) {
                    return (millisecond / MINUTE) + 1;
                }
                return (millisecond / MINUTE);
            }
            case 2: {
                if (yushu_hour != 0) {
                    return (millisecond / HOUR) + 1;
                }
                return (millisecond / HOUR);
            } // hour
            case 3: {
                if (yushu_day != 0) {
                    return (millisecond / DAY) + 1;
                }
                return (millisecond / DAY);
            } // day
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        return result;
    }

    /**
     * 计算时间差
     *
     * @param time时间戳
     * @return 时间差的时间戳
     */
    public static long countTimeDiff(long time) {

        return System.currentTimeMillis() - time;
    }

}

```

