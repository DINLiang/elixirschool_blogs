---
author: Sean Callan
author_link: https://github.com/doomspork
categories: general
date: 2018-10-22
tags: ['umbrella applications', 'software design']
layout: post
title: 'umbrellas: only when it rains?'
excerpt: >
  A look at umbrella applications and how they can help us write cleaner maintainable code.
---

# 雨伞：只有在下雨的时候才用？

umbrella 应用是许多用户不熟悉的功能强大的工具。

在本文中，我们将研究 umbrella 应用，以及为什么我们可能要考虑在下一个项目中使用它们。
## 项目类型

在了解何时使用 umbrella 应用及其作用之前，让我们回顾一下可用的项目类型。

出于所有目的，Erlang 和 Elixir 生态系统中有三种项目类型：应用程序，库和 umbrella，它由前两个组成。

库是可被其他项目和应用程序重复使用的代码包。

在 Elixir 中，库将在 Mix `application/0` 函数中缺少 `:mod` 键，并且还将缺少监督树。

应用程序类似于库，但包含一个监督树。

如果打开项目的 `mix.exs` 文件，并看到 `application/0` 函数定义了 `:mod` 入口点：您正在使用 Application。

这是一个依赖关系，除了进程外，它还具有状态并维护状态。

应用程序不仅仅是具有简单功能的模块。

最后，我们拥有 umbrella 应用，它是一个语法糖，可以将一个库和应用程序集合作为一个项目进行管理。

我们今天将在这里探讨为什么要使用 umbrella 应用。

## 认识 umbrella  应用

如果您还没有的话，请看一下[umbrella projects](/en/lessons/advanced/umbrella-projects/) 课程，其中提供了有关 umbrella 应用的详尽概述。

## 分离关注点

依我的拙见，umbrella 应用的一大优点是强制分离关注点和解耦代码。

我们必须明确我们各个应用的配置，这包括对其他同行应用的依赖。

如果我们的展现层没有内部数据应用作为依赖，这两个组件就没有机会耦合在一起。

虽然理论上使用单一的 Application 也可以实现同样的分离，但 umbrella 应用给我们带来了额外的好处，即通过依赖关系来执行。

我从未见过 "理论" 在专业环境中站立起来，当技术水平和经验各不相同时，umbrella 应用可以消除任何问题和困惑。

除开其他原因，这往往是我是否利用 umbrella 应用的决定性因素。

## 面向服务的架构精简版

如果我们知道我们正在使用内部组件构建一个复杂的应用程序，并且内部组件的大小将随着角色和职责的不同而增长，那么 umbrella 应用就可以成为最有用的工具。

有了一个代码库和一个存储库，我们就可以维护任何数量的子应用程序，这是我们总体 umbrella 应用的一部分。

如果我们要建立在线市场，则可以使用它来保持付款交易代码与其他模块隔离。

我们可以将与第三方服务的集成作为独立的应用程序实现在我们的 umbrella 应用之内。

这不仅使这些关注点分离开来，而且使我们能够分别处理组件，更重要的是：分开测试。

没有什么可以阻止我们在一个 umbrella 应用中运行多个 Phoenix 应用程序。

我们可以独立于面向客户的站点来开发管理 Web 门户，并在单独的端口上运行，这使得网络配置成为问题，从而仅限制对 VPN 用户的访问。

如果应用程序的范围太大，我们需要将其从 umbrella 应用中删除，那就没问题了！

由于存在这些明确的依赖关系，因此使 umbrella 很容易做到这一点，更新这些引用是我们所要做的。

将我们的项目开发为不同的微服务的另一个好处是：大型团队可以在一个代码库上同时工作，同时最大程度地减少代码冲突。

## 一个部署就能解决所有问题

哪个更容易：应用程序部署更改 12 个还是 1 个？

有了 umbrella 应用，发布不仅更容易，而且更强大。

我们不仅有一个单一的工件可以部署，我们只有一个服务可以用 `systemd`（或你选择的工具）来管理。

还有一个额外的好处是，发布我们不同的应用程序不需要复杂的协调。

这还不是全部。 有了 Distillery，我们可以配置发布工件，以包含我们所有的应用程序或只是一个子集。

想一想：我们仍然受益于在单一代码库中工作，但我们的应用分离允许我们部署这些应用的任何组合（只要所有的依赖关系都被考虑在内）。

我们的发布可以作为一个单一的工件开始生活，并根据需要，我们可以分解出组件进行独立发布。

## 撑伞还是不撑伞

与某些人所认为的相反，umbrella 应用并不是针对每个问题的完美解决方案。

在我们 `mix new` 之前，我们应该暂停并考虑一些简单的规则，以帮助指导我们的决策过程：

- 您的应用范围可能会迅速增长吗？
- 我们的应用是否包含多个不同且独立的内部组件？
- 是否会有多个人同时处理代码的不同部分？

如果我们对以上回答是 YES，那么为您的项目选择 umbrella 应用是正确的选择。

我们希望听到您的想法！

您正在使用 umbrella 应用吗？

您是否发现它们有帮助或是有阻碍？

寻找我们将来的博客文章，这些文章将涵盖如何设计和构建您的应用！