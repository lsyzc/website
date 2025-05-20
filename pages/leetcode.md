---
title: LeetCode Problems
description: Some of my thoughts on my mental health during my journey in Open Source
date: 2024-03-16T12:00:00.000+00:00
lang: en
duration: 25min
---

[[toc]]
🧠 Welcome to my LeetCode problem archive!


This page compiles my LeetCode problem-solving records, with a sidebar menu for quick navigation to each problem, making it easier to review and reference.


🗂️ Use the sidebar to navigate through problems with ease~ 🐾
Happy coding! 💻💡


# Hello,world

# Nice to meet you


# 单调栈
## 

```java
#https://www.nowcoder.com/practice/2a2c00e7a88a498693568cef63a4b7bb
import java.io.*;

public class Main {
    public static int MAXN = 1000001;
    public static int n,r;
    public static int[] arr = new int[MAXN];
    public static int[] stack = new int[MAXN];
    public static int[][] ans = new int[MAXN][2];


    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StreamTokenizer in = new StreamTokenizer(br);
        PrintWriter out = new PrintWriter(new OutputStreamWriter(System.out));
        while(in.nextToken() != StreamTokenizer.TT_EOF) {
            n = (int) in.nval;
            for(int i = 0; i < n; i++) {
                in.nextToken();
                arr[i] = (int) in.nval;
            }
        }
        compute();

        for (int i = 0; i < n; i++) {
            out.println(ans[i][0] + " " + ans[i][1]);
        }
        out.flush();
        out.close();
        br.close();



    }
    public static void compute() {
        r = 0;
        int cur;
        for(int i = 0; i < n; i++) {
            while(r > 0 && arr[i] <= arr[stack[r-1]]) {
                cur = stack[--r];
                ans[cur][0] = r > 0 ? stack[r-1] : -1;
                ans[cur][1] = i;
            }
            stack[r++] = i;
        }
        while(r > 0) {
            cur = stack[--r];
            ans[cur][0] = r > 0 ? stack[r-1] : -1;
            ans[cur][1] = -1;
        }

        for(int i = n - 2; i >=0; i--) {
            if(ans[i][1] !=-1 && arr[i] == arr[ans[i][1]] ) {
                ans[i][1] = ans[ans[i][1]][1];
            }
        }
    }
}

```


<details open>
  <summary>测试折叠内容</summary>

  这里是被折叠的内容，可以包含 **Markdown** 格式。
  
  - 列表项
  - 支持图片、代码等

</details>

