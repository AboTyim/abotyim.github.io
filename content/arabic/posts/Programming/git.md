---
title: "إدارة الشيفرات البرمجية Git"

description: "شرح كيفية التعامل مع أوامر Git."

summary: "شرح مفصل لكيفية التعامل مع أوامر Git."

slug: "git"
aliases:
- /Git-Arabic/
- إدارة الشيفرة البرمجية بواسطة Git

author: ["AboTyim"]
date: "2022-03-21"
lastmod: "2022-03-31"
tags: ["programming", "git", "code"]
categories: ["programming"]
series: 
- "مسار مبرمج بايثون"


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
    image: "posts/programming/git.jpeg"
    alt: "Version Control with Git"
    caption: "مصدر الصورة [linkedin.com](https://www.linkedin.com/learning/programming-foundations-version-control-with-git/what-is-git-branching)"
    relative: false # when using page bundles set this to true
    hidden: false # only hide on current single page


weight: 1
---



بسم الله الرحمن الرحيم، الحمد لله الذي علم بالقلم، علم الإنسان مالم يعلم والصلاة والسلام على خير معلم الناس الخير محمد أما بعد:



## مقدمة:

عملية كتابة الكود البرمجي تمر بعدة مراحل وهي الكتابة والتعديل والمسح و إضافة المزيد من الأكواد وهكذا، وتصل لمرحلة ما، تود أن تتراجع عن هذه التعديلات أو تريد معرفة التعديلات التي أجريت على الملف والتراجع عن تعديل بسيط مثلًا، تعتبر هذه الأشياء من أبسط الأمور التي يقدمها لك `git`.

> يمكن النظر له  بشكل مبسط على أنه ((قاعدة بيانات **Database**)) أو نظام ملفات ((**File System**)) يقوم بتخزين العمليات التي نقوم بها على الملف أو مجموعة الملفات بحيث يمكنك العودة لتلك التعديلات في أي وقت. [^خوارزميون]

[^خوارزميون]: [خوارزميون: المبرمجون وإدارة الشيفرات البرمجية Git.](http://lgrth.me/gitbook)


<br>

### مخطط سير عمل **Git Workflow**:

1. يقوم المبرمج بتعديل الملفات في مجلد المشروع الذي تعمل عليه.
2. بعد ذلك يقوم بإضافة الملفات التي تم تعديلها إلى منطقة **Staging Area**: وهي المنطقة التي تنتقل إليها التعديلات التي قام بها المبرمج قبل تخزينها بشكل نهائي.
3. يقوم بعمل **commit**، والتي بدورها تقوم بأخذ جميع التعديلات الموجودة في **Staging Area** ومن ثم تخزينها بشكل دائم إلى المستودع **Repository**.

<br>

### الأمر `init`:

يستخدم لإنشاء مستودع ‫‪**Repository**‬‬ جديد، وهو المكان الي سيقوم `git` بتخزين قائمة التعديلات فيه للمشروع، يجب أن نكون في مجلد المشروع الرئيسي وكتابة الأمر التالي.

```bash
git init
```

<mark>ملاحظة:</mark> إنشاء المستودع الفارغ لايعني أنه سيقوم بتخزين الملفات أو التعديلات في مشروعك.

<br>

### الأمر `config`:

إضافة معلوماتك على **git**، لكي يظهر هوية المستخدم الذي قام بالتعديلات:

```bash
git config --global user.email "myemail@email.com"
git config --global user.name "myname"
```

<br>

### الأمر `add`:

يستخدم لمتابعة التعديلات على المف ونقله من حالة **Untracked** إلى حالة **Tracked** وله استخدام آخر هو نقل الملفات التي تم تعديلها من منطقة **Staging Area** إلى المستودع **Repository** بشكل دائم.

```bash
git add views.py
git add models.py
git add urls.py
```

لمتابعة التعديلات على المجلد كامل:

```bash
git add .
```

<mark>ملاحظة:</mark> سيتم متابعة التعديلات التي تجري على كافة الملفات في المجلد الحالي الذي نفذت فيه هذا الأمر.

<br>

### الأمر `status`:

يستخدم الأمر `status` لمعرفة الملفات التي تم التعديل عليها، ومعرفة الفرع **branch** الحالي.

```bash
git status
```

<br>

### الأمر `commit`:

هذا الأمر ببساطة هو الذي يلتقط الصورة أو حالة الملفات والخيار **m** خاص بوضع رسالة يقوم المبرمج بوضعها لتحفظ مع تلك الصورة، وهي توضح سبب التعديلات أو الخطأ الذي تم إصلاحه أو أي رسالة تبين الهدف من تلك التعديلات.
أما الخيار **a** يستخدم لإضافة الملفات التي تم التعديل عليها إلى منطقة **Staging Area** ثم عمل **commit** للملفات ونقلها إلى المستودع **Repository**.

```bash
‫‪git‬‬ ‫‪commit‬‬ ‫‪-m‬‬ ‫‪'My‬‬ ‫'‪message‬‬
‫‪git‬‬ ‫‪commit‬‬ ‫‪-a -m‬‬ ‫‪'My‬‬ ‫'‪message
```

<br>

### الأمر `diff`:

يستخدم لإظهار التعديلات التي أجريت على الملفات الحالية ومقارنتها بآخر **commit**:

```bash
git diff
```

<br>

### الأمر `reset`:

عند استخدام `add`، فنحن نقوم بنقل التعديلات إلى **Staging Area** لتصبح جاهزة لتخزينها بشكل دائم في المستودع من خلال `commit`، وهنا قد يحتاج المبرمج إلى إعادتها من تلك المنطقة، أي ما قبل `add`، أي التراجع عن التعديلات التي قان بها المبرمج، أي تتحول حالة الملف أو مجموعة الملفات من **Unstaged** إلى **Modified**.

```bash
git reset HEAD models.py
```

يمتلك الأمر `reset` أحد الخيارات المسماة `--hard`، واستخدام هذا الخيار قد يكون خطير للغاية كونه قد يتسبب في التراجع عن التعديلات التي قمت بها ليس فقط من **Staging Area** بل من **Repository**.

<br>

### الأمر `checkout`:

قد تقوم بإجراء تعديلات على ملف أو مجموعة ملفات، وبعد فترة تكتشف لأي سبب أنك تريد إلغاء كل تلك التعديلات والعودة إلى الوضع التي كانت عليه تلك الملفات قبل إجراء تلك التعديلات الأخيرة أي عودة الملف لحالته لآخر **commit**، هنا يوفر **git** أسلوب مبسط لإجراء تلك العملية.

```bash
git checkout models.py
```

<mark>ملاحظة:</mark> يستخدم الأمر `checkout` أيضًا للتنقل من **Branch** لآخر.

الإنتقال لفرع موجود مسبقًا بشكل محلي:

```bash
git checkout {{branch_name}}
```

إنشاء فرع **Branch** جديد والإنتقال إليه بشكل مباشر:

```bash
git checkout -b {{branch_name}}
```

<br>

### الأمر `branch`:

عرض كافة الفروع **Branches** الحالية للمستودع المحلي:

```bash
git branch
```

عرض كافة الفروع **Branches** الحالية للمستودع المحلي والبعيد:

```bash
git branch --all
git branch -a
```

إنشاء فرع **Branch** جديد:

```bash
git branch {{branch_name}}
```

إعادة تسمية فرع **Branch**:

```bash
git branch -m {{old_branch_name}} {{new_branch_name}}
```

حذف فرع **Branch**:

```bash
git branch -d {{branch_name}}
```

<br>

### الأمر `merge`:

دمج فرع **Branch** إلى الفرع الحالي:

```bash
git merge {{branch_name}}
```

<br>

### الأمر `rm`:

لحذف ملف بشكل مباشر من مجلد المشروع ومن المستودع:

```bash
git rm models.py
```

لحذف مجلد بشكل مباشر من المشروع ومن المستودع:

```bash
git rm -r utils
```

لحذف ملف من git كمتابعة فقط، أي من **Staged Area** ولكن لا تريد حذفه من مجلد المشروع:

```bash
git rm --cached models.py
```

<br>

### الأمر `mv`:

يستخدم لنقل الملفات من مجلد إلى مكان آخر داخل مشروعك:

```bash
git mv {{path/to/file}} {{new/path/to/file}}
```

يستخدم أيضًا لإعادة تسمية الملفات:

```bash
git mv {{filename}} {{new_filename}}
```

<br>

### الأمر `log`:

 لرؤية سجل العمليات التي حدثت على المستودع الذي تعمل عليه:

```bash
git log
```

سيتم عرض عدد **commits** التي تمت على المستودع الذي تعمل عليه، بالإضافة إلى تفاصيل كل **commit** من خلال عرض رقمها ومن قام بها والتاريخ والرسالة التي توضح سبب أو وصف **commit**، شيء مشابه لما يلي:

```
commit e524852999ed457e7e888c44f51dd9222bcb34d7
Author: Mr.Ahmed <xyz@gmail.com>
Date:   Mon Apr 12 22:14:11 2022 +0300

    update page about.md

```

لرؤية **commits** التي حدثت للمشروع بشكل مختصر:

```bash
git log --oneline
```

لرؤية **commits** التي حدثت على ملف أو المجلد:

```bash
git log {{path/to/file_or_directory}}
```

لمعرفة التفاصيل بشكل أكبر على كل commit:

```bash
git log -p
```

عرض الإحصائيات بشكل مختصر:

```bash
git log --stat
```

عرض معلومات لعدد محدد من **commits**:

```bash
git log -n {{number}}
```

للبحث برسائل **commits** التي تحتوي على نص معين:

```bash
git log -i --grep {{search_string}}
```

<br>

### الأمر `remote`:

لعرض المستودعات التي نتعامل معها عن بعد:

```bash
git remote -v
```

لعرض معلومات المستودع البعيد:

```bash
git remote show {{remote_name}}
```

لتغير اسم مستودع بعيد:

```bash
git remote rename {{old_name}} {{new_name}}
```

لحذف مستودع بعيد:

```bash
git remote remove {{remote_name}}
```

لإضافة مستودع بعيد جديد:

```bash
git remote add {{remote_name}} {{remote_url}}
```

لتغير رابط مستودع بعيد:

```bash
git remote set-url {{remote_name}} {{new_url}}
```

<br>

### الأمر `clone`:

لنسخ مشروع بشكل كامل والحصول على نسخة من المستودع بشكل خاص:

```bash
git clone {{remote_repository_location}}
```

<br>

### الأمر `pull`:

لتحميل نسخة التغيرات التي حصلت في المستودع البعيد مع عمل دمج لها:

```bash
git pull
```

<br>

### الأمر `push`:

لرفع التعديلات الموجودة في المستودع المحلي **Local Repository** الموجود في جهازك إلى المستودع البعيد:

```bash
git push {{remote_name}} {{local_branch}}
```

<br>

### الأمر `stash`:

يستخدم لحفظ حالة المستودع الحالية، قد تود حفظ حالة المشروع والإنتقال لفرع آخر **Branch** دون أن تقوم بعمل **commit** لأنك لم تنهي التعديلات الحالية فهنا بإمكانك استخدام **Stash**:

```bash
git stash
```

<mark>ملاحظة:</mark> تستطيع تخزين أكثر من حالة.

لمعرفة قائمة الحالات التي قمت بتخزينها لكي تساعدك في الرجوع للحالة التي تريدها:

```bash
git stash list
```

للرجوع لحالة مخصصة سابقة بعد مشاهدة قائمة الحلات من خلال الأمر السابق:

```bash
git stash apply stash@{2}
```

لحذف كل الحالات:

```bash
git stash clear
```

<br>

### الأمر `clean`:

قد تحتاج أحيانًا إلى إزالة جميع الملفات التي تكون حالتها **Untracked** في مشروعك وغير موجودة في ملف `.gitignore`:

```bash
git clean -f
```

الأمر التالي نفس الأمر السابق ولكن مع حذف المجلدات:

```bash
git clean -f -d
git clean -fd
```

لحذف الملفات بشكل تفاعلي:

```bash
git clean -i
```

لحذف المجلدات بشكل تفاعلي:

```bash
git clean -i -d
```

لإظهار الملفات التي يستم حذفها ولكن دون اتخاذ أي إجراء:

```bash
git clean --dry-run
git clean --dry-run -f

git clean -n
git clean -n -f
```

لإظهار المجلدات التي يستم حذفها ولكن دون اتخاذ أي إجراء:

```bash
git clean --dry-run -d
git clean -n -d
```

<br>

### الأمر `tag`:

يستخدم لإنشاء تسميات للنسخ أو لإصدارات الكود.

عرض كافة الوسوم:

```bash
git tag
```

إنشاء تسمية (وسم) مع وضع رسالة:

```bash
git tag {{tag_name}} -m {{tag_message}}
```

لحذف تسمية موجودة مسبقًا:

```bash
git tag -d {{tag_name}}
```

<br>

## المراجع:

- [خوارزميون: المبرمجون وإدارة الشيفرات البرمجية Git.](http://lgrth.me/gitbook)
- [Flex Courses: دورة إدارة نسخ البرمجة باستخدام git.](https://www.flexcourses.com/courses/git-basics/)