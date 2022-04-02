---
date: 2021-10-26T16:22:00.877Z
updated: null
title: 让 Typecho 支持 QQ 头像
slug: '44'
categories: 技术
type: post
permalink: posts/44
---


# 前言
Typecho 使用的是 Gravatar 作为用户的头像，但是大部分用户都没有设置 Gravatar 头像，而且 Gravatar 在国内访问速度感人，大大影响了用户体验和站点速度。

# 解决方法
让数字 QQ 邮箱显示 QQ 头像，非数字 QQ 邮箱使用 Gravatar 国内镜像加快速度。

# 修改文件
在 typecho 根目录 /var/Typecho/Common.php 文件，注释掉大约在第 1010 行至第 1028 行的代码

    /**
    public static function gravatarUrl($mail, $size, $rating, $default, $isSecure = false)
    {
    ……
    return $url;
    }**/
在其下方添加以下 php 代码

    public static function gravatarUrl($mail, $size, $rating, $default, $isSecure = false)
    {
      $reg = "/^\d{5,11}@[qQ][Qq]\.(com)$/";
      if (preg_match($reg, $mail)) {
        $img    = explode("@", $mail);
        $url = "//q2.qlogo.cn/headimg_dl?dst_uin={$img[0]}&spec=100";
      } else {
        if (defined('__TYPECHO_GRAVATAR_PREFIX__')) {
          $url = __TYPECHO_GRAVATAR_PREFIX__;
        } else {
          $url = $isSecure ? 'https://gravatar.loli.net' : 'http://gravatar.loli.net';
          $url .= '/avatar/';
        }
        if (!empty($mail)) {
          $url .= md5(strtolower(trim($mail)));
        }
        $url .= '?s=' . $size;
        $url .= '&amp;r=' . $rating;
        $url .= '&amp;d=' . $default;
      }
      return $url;
    } 
现在 Typecho 全局支持数字 QQ 邮箱显示 QQ 头像了，Gravatar 使用国内镜像源。