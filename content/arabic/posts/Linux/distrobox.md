---
title: "تشغيل أي توزيعة لينكس بواسطة DistroBox"

description: "هي أداة تسمح لك بإنشاء وإدارة الحاويات على توزيعة لينكس."

summary: "هي أداة تسمح لك بإنشاء وإدارة الحاويات على توزيعة لينكس."

slug: "distrobox"
aliases: 
- "/linux/distrobox/"

author: ["AboTyim"]
date: "2022-04-25"
lastmod: "2022-04-26"
tags: ["linux", "containers", "docker", "podman", "distrobox"]
categories: ["containers"]
# series: 
# - "مسار سطر أوامر لينكس"

hidemeta: false
hideSummary: false
disableHLJS: false # to disable highlightjs
disableShare: false
#canonicalURL: "https://canonical.url/to/page"

ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
searchHidden: false
ShowToc: true
TocOpen: true
comments: true

cover:
    image: "posts/linux/distrobox/distrobox.png"
    alt: "manage containers"
    caption: "مصدر الصورة [distrobox.privatedns.org](https://distrobox.privatedns.org/)"
    relative: false # when using page bundles set this to true
    hidden: false # only hide on current single page


weight: 1
---



بسم الله الرحمن الرحيم، الحمد لله الذي علم بالقلم، علم الإنسان مالم يعلم والصلاة والسلام على خير معلم الناس الخير محمد أما بعد:



## مقدمة:

هي أداة تسمح لك بإنشاء وإدارة الحاويات على توزيعة لينكس المفضلة لديك باستخدام `Docker` أو `Podman`. تصبح الحاوية التي تم إطلاقها متكاملة بشكل كبير مع النظام الأساسي (المضيف)، مع إمكانية مشاركة دليل المستخدم **HOME** جنبًا إلى جنب مع أقراص التخزين الخارجي وأجهزة **USB** والتطبيقات الرسومية.
يقوم **DistroBox** باستخدام الصور المتوفر على **Docker Hub** أو مستودع فيه صور **Images** لكي يتم إنشاء منها حاويات.



## المتطلبات قبل البدء:

يجب تثبيت `Docker` أو `Podman` لكي يتم التعامل مع الحاويات وإدارتها في الخلفية:
- أقل إصدار من `Docker` يجب أن يكون 18.06.1.
- أقل إصدار من `Podman` يجب أن يكون 2.1.0.



## تثبيت DistroBox على التوزيعات الديبيانية:

```shell
curl -s https://raw.githubusercontent.com/89luca89/distrobox/main/install | sudo sh
```



## إلغاء تثبيت DistroBox على التوزيعات الديبيانية:

```shell
curl -s https://raw.githubusercontent.com/89luca89/distrobox/main/uninstall | sudo sh
```



## إنشاء حاوية من صورة:

```shell
distrobox-create --name container-name --image os-image:version
```


في المثال التالي سيتم إنشاء حاوية من صورة كالي لينكس:

```shell
distrobox-create --image kalilinux/kali-last-release --name kalilinux
```



## عرض قائمة الحاويات التي تم إنشاءها بواسطة DistroBox:

```shell
distrobox-list
```



## الدخول لحاوية تم إنشاءها:

```shell
distrobox-enter --name container-name
```


للدخول إلى حاوية كالي لينكس التي تم إنشاءها مسبقًا:

```shell
distrobox-enter --name kalilinux
```



## تنفيذ الأوامر بداخل حاوية DistroBox:

```shell
distrobox-enter --name container-name  -- command
```


في المثال التالي سيتم تحديث المستودعات:

```shell
distrobox-enter --name kalilinux -- sudo apt update
```



## تصدير التطبيقات من الحاوية إلى المضيف:

```shell
distrobox-enter --name container-name
distrobox-export --app appname
```

في البداية يجب الدخول إلى الحاوية التي نريد تصدير التطبيق منه:

```shell
distrobox-enter --name kalilinux
```

بعد ذلك قم بتثبيت التطبيق المراد تصديره، هنا في مثالنا التالي نريد تصدير تطبيق `flameshot`:

```shell
sudo apt install flameshot
```

أمرتصدير تطبيق `flameshot`:

```shell
distrobox-export --app flameshot
```

للخروج من الحاوية الحالية:

```shell
logout
```

بعد ذلك سنجد تطبيق flameshot في النظام الأساسي (المضيف).



## إيقاف حاوية تعمل في الخلفية:

```shell
distrobox-stop container-name
```


إيقاف حاوية كالي لينكس التي تعمل في الخلفية:

```shell
distrobox-stop kalilinux
```



## حذف حاوية موجودة بشكل سابق:

```shell
distrobox-rm container-name
```

```shell
distrobox-rm kalilinux
```



## إنشاء نسخة عن حاوية موجودة بشكل سابق:

<mark>تنويه</mark>: قبل ذلك يجب أن يتم إيقاف الحاوية المراد إنشاء نسخة عنها، وتم شرح ذلك أعلاه.

```shell
distrobox-create --name new-container-name --clone container-name
```

```shell
distrobox-create --name kalilinux-clone --clone kalilinux
```



## المراجع:

- [Run Any Linux Distribution Inside Linux Terminal.](https://www.tecmint.com/distrobox-run-any-linux-distribution/)
- [Use any linux distribution inside your terminal.](https://distrobox.privatedns.org/)
- [Run ANY Linux App on ANY Distro! (No VM) | Distrobox Tutorial.](https://www.youtube.com/watch?v=x5sAY8D28fA)
