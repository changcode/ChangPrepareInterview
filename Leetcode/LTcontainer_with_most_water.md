# Container With Most Water

Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container.

暴力肯定超时O(n<sup>2</sup>)
分析：

S(l, r) = min(h[l], h[r]) * (r - l);

(r - l) Max when r = len, l = 0;

If we want S'(l', r') > S(l, r) 

there must another min'(h[l'], h[r']) > min(h[l], h[r])

So we can make the l to right until h[l'] > h[l], at h[l] < h[r] case;

also we can make r to left until h[r'] > h[r], at h[r] < h[l] case.


```java
public int maxArea(int[] height) {
        if (height == null || height.length < 2) {
            return 0;
        }
        int ans = 0;
        
        int l = 0, r = height.length - 1;
        while(l < r) {
            // System.out.println(l + ", " + r);
            int h = Math.min(height[l], height[r]);
            int area = h * (r - l);
            ans = Math.max(area, ans);
            if (l == r - 1)
               break;
            if (height[l] < height[r]) {
                int lh = height[l];
                while( l < r && lh >= height[l]) {
                    l++;
                }
            } else {
                int rh = height[r];
                while( l < r && rh >= height[r]) {
                    r--;
                }
            }
            
        }
        
        return ans;
    }
```