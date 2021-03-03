---
title: Running the Sample Program
author: lijiabao
date: 2020-12-06 01:49:43.261027000 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# Running the Sample Program

The internationalized program is flexible; it allows the end user to specify a language and a country on the command line. In the following example the language code is `fr` (French) and the country code is `FR` (France), so the program displays the messages in French:

```

% java I18NSample fr FR
Bonjour.
Comment allez-vous?
Au revoir.

```

In the next example the language code is `en` (English) and the country code is `US` (United States) so the program displays the messages in English:

```

% java I18NSample en US
Hello.
How are you?
Goodbye.

```
