---
layout: post
author: "Hongil Son"
title:  BOJ_19590_버드맨
subtitle: Greedy
type: "Baekjoon Online Judge"
text: true
book: true
order: 1
data-name: 1-4-2021
ridibooks: true
---


# 문제
<img src="img/19590.jpg" width="100%">
- 종류가 다른 구슬끼리 부딫히면 깨지고 남는 구슬이 최소가 되도록 만드는 것이 문제다.<br>

# 해결 과정
- 이 문제를 읽자마자 그리디 알고리즘을 이용해서 풀면 되겠다고 생각했다.<br>
그래서 정말 정직하게 구슬들을 갯수를 기준으로 정렬한 다음 갯수가 가장 많은 것부터 그 다음으로 많은 구슬끼리 부딫혀 제거해 나가는 방법으로 코드를 적었다.<br>
하지만 빈번히 시간 초과가 발생하였고 정말 오랜 시간 생각 끝에 가장 많은 구슬의 갯수가 나머지 구슬보다 크면 maximum-extra_bead<br>
그렇지 않은 경우 어떤 경우라도 전체 구슬 갯수가 짝수면 0 홀수면 1이라는 규칙을 알았다.<br>
규칙을 찾고 코드를 적으니 굉장히 허무했던 문제였다.


```cpp
#include <iostream>
#include <algorithm>

using namespace std;

int main(void){
  ios_base::sync_with_stdio(false);
  cin.tie(NULL);
  cout.tie(NULL);

  int N;
  cin >> N;

  int maximum = 0;
  long long all_bead = 0;

  for(int i = 0; i < N; i++){
    int a;
    cin >> a;
    
    all_bead = all_bead + a;
    maximum = max(a, maximum);
  }

  long long extra_bead = all_bead-maximum;
  long long ans;

  if(maximum > extra_bead){
    ans = maximum - extra_bead;
  }
  else{
    if(all_bead % 2 == 0){
      ans = 0;
    }
    else{
      ans = 1;
    }
  }

  cout << ans << '\n';

  return 0;
}
```