---
layout: post
title:  spring mvc web - 字典工具类
categories: JAVA
description: 字典工具类
keywords: java, spring web, 字典
---

已下工具类

```
/**
 * 字典工具类
 */
public class DictUtils {
	
	private static SuperDao superDao = SpringContextHolder.getBean(SuperDao.class);

	public static final String CACHE_DICT_MAP = "dictMap";

    public static String getDictLabel(String value, String type, String defaultValue){
		if (StringUtils.isNotBlank(type) && StringUtils.isNotBlank(value)){
			for (Dict dict : getDictList(type)){
				if (type.equals(dict.getType()) && value.equals(dict.getValue())){
					return dict.getLabel();
				}
			}
		}
		return defaultValue;
	}

	public static String getDictValue(String label, String type, String defaultLabel){
		if (StringUtils.isNotBlank(type) && StringUtils.isNotBlank(label)){
			for (Dict dict : getDictList(type)){
				if (type.equals(dict.getType()) && label.equals(dict.getLabel())){
					return dict.getValue();
				}
			}
		}
		return defaultLabel;
	}
	
	public synchronized static Map<String, List<Dict>> getAllDistMap(){
		@SuppressWarnings("unchecked")
        Map<String, List<Dict>> dictMap = (Map<String, List<Dict>>)CacheUtil.get(CACHE_DICT_MAP);
        if(dictMap == null || dictMap.isEmpty()){
        	dictMap = Maps.newHashMap();
	        List<Dict> findAllList =  superDao.where("delFlag",Dict.DEL_FLAG_NORMAL).asc("sort").queryList(Dict.class);
	        for (Dict dict : findAllList){
	            List<Dict> dictList = dictMap.get(dict.getType());
	            if (dictList != null){
	                dictList.add(dict);
	            }else{
	                dictMap.put(dict.getType(), Lists.newArrayList(dict));
	            }
	        }
	       CacheUtil.put(CACHE_DICT_MAP, dictMap);
        }
        return dictMap;
	}
	
	public static List<Dict> getDictList(String type){
		Map<String, List<Dict>> dictMap = getAllDistMap();

        List<Dict> dictList = dictMap.get(type);
        if (dictList == null){
            dictList = Lists.newArrayList();
        }
        
        return dictList;
	}
	
}
``