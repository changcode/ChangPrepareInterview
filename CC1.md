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


普通模拟解法String操作时间复杂度过高 Time O(p + k <sup>2</sup> ) P是str length，k是character sequence

使用StringBuffer

##6.顺时针旋转matrix 90度


##7.把Matrix中含有0的行列都设为0
两个方法：
1.2个vector，存储含有0的行和列，之后设置。
```java
public void setZeroN(int[][] matrix) {
    boolean[] row = new boolean[matrix.length];
    boolean[] col = new boolean[matrix[0].length];

    for (int i = 0; i < matrix.length; i++) {
        for (int j = 0; j < matrix[0].length; j++) {
            if(matrix[i][j] == 0) {
                row[i] = true;
                col[j] = true;
            }
        }
    }

    for (int i = 0; i < row.length; i ++) {
        if (row[i])
            zeroMatrixRow(matrix, i);
    }

    for (int i = 0; i < col.length; i++) {
        if (col[i])
            zeroMatrixCol(matrix, i);
    }
}
```
2.在matrix首行，首例存储含有0的行列。
```java
    public void setZeros(int[][] matrix) {
        int row = matrix.length;
        int col = matrix[0].length;

        boolean rowZero = false;
        boolean colZero = false;

        for (int i = 0; i < row; i++) {
            if (matrix[i][0] == 0)
                rowZero = true;
        }

        for (int i = 0; i < col; i++) {
            if (matrix[0][i] == 0)
                colZero = true;
        }

        for (int i = 1; i < row; i ++) {
            for (int j = 1; j < col; j++) {
                if (matrix[i][j] == 0) {
                    matrix[0][j] = 0;
                    matrix[i][0] = 0;
                }
            }
        }

        for (int i = 1; i < col; i ++) {
            if (matrix[0][i] == 0)
                zeroMatrixCol(matrix, i);
        }

        for (int i = 1; i < row; i++) {
            if (matrix[i][0] == 0)
                zeroMatrixRow(matrix, i);
        }

        if (rowZero)
            zeroMatrixCol(matrix, 0);

        if (colZero)
            zeroMatrixRow(matrix, 0);
    }

    public void zeroMatrixCol(int[][] matrix, int col) {
        for (int i = 0; i < matrix.length; i++) {
            matrix[i][col] = 0;
        }
    }

    public void zeroMatrixRow(int[][] matrix, int row) {
        for (int i = 0; i < matrix[0].length; i++) {
            matrix[row][i] = 0;
        }
    }
```

##8.Give a method isSubstring check one string is another. Check s2 is a rotation of s1 only can use isSubstring.("waterbottle" rotation is "erbottlewat")

s1 = xy = "shuchang"
x = "shu"
y = "chang"

s2 = yx = changshu

Tricky: s1s1 = xyxy
isSubstring(s1s1, s2) is true;
