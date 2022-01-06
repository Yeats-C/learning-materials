# 获得map中最大的value以及最大的key

```
import java.util.Arrays;
import java.util.Collection;
import java.util.HashMap;
import java.util.Map;
import java.util.Set;
 
public class MaxMapDemo {
 
public static void main(String[] args) {
Map<Integer, Integer> map = new HashMap<Integer, Integer>();
map.put(1, 8);
map.put(3, 12);
map.put(5, 53);
map.put(123, 33);
map.put(42, 11);
map.put(44, 42);
map.put(15, 3);
 
System.out.println(getMaxKey(map));
System.out.println(getMaxValue(map));
 
}
 
/**
* 求Map<K,V>中Key(键)的最大值
* @param map
* @return
*/
public static Object getMaxKey(Map<Integer, Integer> map) {
if (map == null) return null;
Set<Integer> set = map.keySet();
Object[] obj = set.toArray();
Arrays.sort(obj);
return obj[obj.size()-1];
}
 
/**
* 求Map<K,V>中Value(值)的最大值
* @param map
* @return
*/
public static Object getMaxValue(Map<Integer, Integer> map) {
if (map == null) return null;
Collection<Integer> c = map.values();
Object[] obj = c.toArray();
Arrays.sort(obj);
return obj[obj.size()-1];
}
 
}
```

[ 参考来源](https://www.cnblogs.com/zlj1992/p/7306776.html)
