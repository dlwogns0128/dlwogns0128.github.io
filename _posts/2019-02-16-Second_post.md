---
layout: posts
title: "[Algorithm, 수학] Broken Calculator"
tag: Algorithm, 수학

---

# Broken Calculator

## 원문
On a broken calculator that has a number showing on its display, we can perform two operations:

-   **Double**: Multiply the number on the display by 2, or;
-   **Decrement**: Subtract 1 from the number on the display.

Initially, the calculator is displaying the number  `X`.

Return the minimum number of operations needed to display the number  `Y`.

## 해석
- `X`에서 `Y`로 갈 수 있는 최소의 연산 수를 계산하자
- 연산은 곱하기 2(**Double**)와 빼기 1(**Decrement**)만 존재한다

## 풀이

 - 처음에는 DP(Dynamic Programming)을 생각해 보았다. 하지만 탐색을 무한히 하게 되어 오류가 남
 - `Y`에서 `X`로 반대로 가는 것을 생각해봄
 - 그러면 연산 방법은 `나누기 2`와 `더하기 1`로 바뀌게 되며 나누기의 경우 1로 수렴되어 바운더리가 만들어지게 된다!
 - 하지만 `Y`에서 나누기와 더하기를 어떻게 조합하는지 생각하다가 못품..

## 해설

```C++

```
- The workspace synchronization will sync all your files, folders and settings automatically. This will allow you to fetch your workspace on any other device.
	> To start syncing your workspace, just sign in with Google in the menu.

- The file synchronization will keep one file of the workspace synced with one or multiple files in **Google Drive**, **Dropbox** or **GitHub**.
	> Before starting to sync files, you must link an account in the **Synchronize** sub-menu.

## Open a file

You can open a file from **Google Drive**, **Dropbox** or **GitHub** by opening the **Synchronize** sub-menu and clicking **Open from**. Once opened in the workspace, any modification in the file will be automatically synced.

