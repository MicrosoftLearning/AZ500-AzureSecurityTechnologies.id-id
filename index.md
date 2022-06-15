---
title: Petunjuk Host Online
permalink: index.html
layout: home
ms.openlocfilehash: 7ad42e4108b96ec131049fa1035ab1d112d95d90
ms.sourcegitcommit: 2eb153f2856445e5afaa218a012cb92e3d48f24b
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 11/16/2021
ms.locfileid: "145195942"
---
# <a name="content-directory"></a>Direktori Konten

File lab yang diperlukan dapat [diunduh di sini](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/archive/master.zip)

Hyperlink ke masing-masing latihan dan demo lab tercantum di bawah ini.

## <a name="labs"></a>Lab

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| Modul | Laboratorium |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
