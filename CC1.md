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
        if ((1 << val & bitVector) > 1)
            return false;
        bitVector = bitVector | 1 << val;
    }
    return true;
}
```