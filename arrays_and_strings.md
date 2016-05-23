# Arrays and Strings
1. 判断一个String的 All Chars 是否Unique, 如果不能使用额外空间求解

`public boolean isUniqueChars(String str) {

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
    }`

2. 

