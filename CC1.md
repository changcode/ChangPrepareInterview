# Arrays and Strings

##1. 判断一个String的 All Chars 是否Unique, 如果不能使用额外空间求解

普通解法，开数组长度128（ASCII），charAt 直接用int接收。
Time:O(n) Space:O(n)

```java
public boolean isUniqueChars(String str) {
    int totalChars = 128;

    if (str.length() > totalChars)
        return false;

    int[] charsArray = new int[totalChars];
    for (int i = 0; i < str.length(); i++) {
        int c = str.charAt(i);
        if (charsArray[c] != 0)
            return false;
        else
            charsArray[c] ++;
    }
    return true;
}
```
 
不使用额外空间解 使用bit vector Time:O(n) Space:O(1)
```java
public boolean isUniqueCharsBit(String str) {
    int bitVector = 0;
    for (int i = 0; i < str.length(); i++) {
        int val = str.charAt(i);
        if ((1 << val & bitVector) > 0) //大于0 不是 1 ！！
            return false;
        bitVector = bitVector | 1 << val;
    }
    return true;
}
```

##2.反转字符串在C／C++ （Char ＊）
注意in place 和 null 处理

[Java 处理方式(Java hungry)](http://javahungry.blogspot.com/2014/12/5-ways-to-reverse-string-in-java-with-example.html)
```C
void reverse(char *str) {
  char *end = str;
  char tmp;
  if(str) {
    while(*end) {
      end++;
    }
  }
  end--;
  while(str < end) {
    tmp = *str;
    *str++ = *end;
    *end-- = tmp;
  }
}
```

##3.Two字符串，判断一个是否为另一个的全排列
1. Case sensitive 
2. whitespace is significant 

*解法1 排序

Time O(nLog(n)) 
```java
public String sort(String s) {
    char[] array = s.toCharArray();
    java.util.Arrays.sort(array);
    return new String(array);
}

public boolean isStringPermutation(String s, String t) {
    if (s.length() != t.length())
        return false;
    return sort(s).equals(sort(t));
}
```

*解法2

HashTable

```java
public boolean isStringPermutationHashtable(String s, String t) {
    HashMap<Character, Integer> hash = new HashMap();

    if (s.length() != t.length())
        return false;

    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        if (hash.containsKey(c)) {
            int num = hash.get(c);
            hash.put(c, num + 1);
        } else {
            hash.put(c, 1);
        }
    }

    for (int i = 0; i < t.length(); i++) {
        char c = t.charAt(i);
        if (hash.containsKey(c)) {
            int num = hash.get(c);
            if (num - 1 < 0)
                return false;
            else
                hash.put(c, num - 1);
        } else {
            return false;
        }
    }
    return true;

}
```

##4.替换字符串中所有的whitespace为％20，java使用char［］代替

解法直接计算新数组长度，backwards方向组合
```java
public String replaceWhitespace(String str) {
    char[] array = str.toCharArray();

    int countSpace = 0;
    for (int i = 0; i < array.length; i++) {
        if (array[i] == ' ')
            countSpace ++;
    }

    int newLength = str.length() + countSpace * 2 + 1;
    char[] newArray = new char[newLength];

    newLength--;
    newArray[newLength--] = '\0';

    for (int i = newLength, j = str.length() - 1; i >= 0; i--, j--) {
        if (array[j] == ' ') {
            newArray[i--] = '0';
            newArray[i--] = '2';
            newArray[i] = '%';
        } else {
            newArray[i] = array[j];
        }
    }

    return new String(newArray);

}
```


##5.压缩字符串，aabcccaaa -> a2b1c3a3, 如果压缩后字符串长度没有减少，输出原始字符串


