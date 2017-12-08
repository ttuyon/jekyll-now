---
layout: post
title: "Angular의 Change Detection에 대해 알아야 하는 것들"
subtitle: "내부 구현과 이용 사례에 대한 탐구"
date: 2017-12-05
author: ttuyon
category: Angular
tags: Angular
finished: false
---

원문: <i class="fa fa-medium"></i> [Everything you need to know about change detection in Angular](https://blog.angularindepth.com/everything-you-need-to-know-about-change-detection-in-angular-8006c51d206f)

# 서문

이 글은 두 부분으로 이루어져 있다. 앞부분은 조금 기술적이고 많은 소스코드들을 포함하고 있다. 여기에선 내부적으로 change detection 메카니즘이 어떻게 작동하는지를 설명한다. 내용들은 모두 최신 Angular 버전인 4.0.1에 기반한다.

뒷부분은 애플리케이션과 그 내용들에서 change detection을 어떻게 사용할 수 있는지를 보여준다.

# 뷰는 핵심 개념이다

angular는 뷰라는 저레벨 추상화를 사용한다. 뷰와 컴포넌트 사이에는 직접적인 관계가 있다. - 하나의 뷰는 하나의 컴포넌트와 관계되어 있으며 반대의 경우도 마찬가지다. 뷰는 `component` 프로퍼티에 자신과 관련한 컴포넌트 클래스의 인스턴스에 대한 참조를 가지고 있다. 프로퍼티 체크와 DOM 업데이트같은 모든 작동들은 뷰 안에서 수행된다, 따라서 앵귤러의 상태는 뷰들의 트리라고 하는것이 기술적으로는 더욱 정확하다, 반면에 컴포넌트는 뷰에 대한 더욱 높은 차원의 컨셉이라고 할 수 있다.

> 뷰는 애플리케이션 UI의 기본 빌딩 블록이다. 뷰는 생성과 파괴를 같이 하는 요소(element)들의 가장 작은 그룹이다.
> 
> 뷰 안의 요소(element)의 속성은 바꿀 수 있다. 그러나 그 구조(갯수와 순서)는 바꿀 수 없다. 요소(element)의 구조를 바꾸는 것은 중첩된 뷰를  ViewContainerRef를 통해 삽입하고 옮기고 지우는 것에 의해서만 가능하다.

# 뷰의 상태

각각의 뷰는 상태를 가지고 있다. 이 상태는 매우 중요한 역할을 한다, 왜냐하면 그(뷰의 상태) 값에 기반하여 angular는 뷰와 뷰의 모든 자식 요소들을 위해 change detection을 수행 할 것인지 아닌지를 결정하기 때문이다. 많은 상태들이 있지만 이 아티클의과 관련된 상태들은 다음과 같다:

1. FirstCheck
2. ChecksEnables
3. Errored
4. Destroyed

`ChecksEnabled`가 `false`이거나 뷰가 `Errored` 또는 `Destroyed` 상태일 때 뷰와 그 자식 뷰들에 대한 change detection은 생략된다. 기본적으로 모든 뷰들은 `ChangeDetectionStrategy.OnPush`가 사용되지 않는 이상 `ChecksEnabled`상태로 초기화 된다. 더 가서, 상태는 합쳐질 수 있다. 예를들면 뷰는 `FirstCheck`와 `CheckesEnabled`플래그가 함께 가지고 있을 수 있다.

angular는 뷰들을 조작하기 위해 많은 고레벨의 개념들을 가지고 있다. 그중의 하나는 `ViewRef`이다. 