---
title: Views
parent: Core Concepts
layout: default
---

# Views
<br/>
### THE LIST VIEW AND THE PAGE VIEW

As discussed in the [**Cloned Pages**](./cloned-pages.html) section, a clonable template acquires a 'split personality'.<br/>
To recap -<br/>
Suppose we have a template named _blog.php_.

The simplest case is when this template has not yet been set as clonable (i.e. having similar pages created out of it). No views are associated with this page. The only possible way to execute this page is -<br/>
_<https://www.yoursite.com/blog.php>_

However, once _blog.php_ is set as being clonable there can be multiple pages that have been cloned out of this template.<br/>
Obviously, we now have atleast two scenarios _blog.php_ may be executed in -

**1\.** _blog.php_ itself is executed -<br/>
_<https://www.mysite.com/blog.php>_ or<br/>
_<https://www.mysite.com/blog/>_ (if pretty-urls activated)

**2\.** A page created out of blog.php is executed -<br/>
_<https://www.mysite.com/blog.php?p=12>_ or<br/>
*<https://www.mysite.com/blog/some_page_name.html>* (if pretty-urls activated)

In the first scenario, _blog.php_ is said to be executing in **list view** while in the second case it is said to be executing in **page view**.

The **page view** is expected to show details relevant to the page being executed and hence makes available the pertinent variables.<br/>
The **list view **is expected to act like a listing page (e.g. list all the pages belonging to the blog etc.) and has a different set of variables available for this purpose.

<p class="notice">
    When we say 'is expected to', that is exactly what we mean.<br/>
    Couch simply sets certain variables to make it known to your script which view it is executing in. It is you who decides what to display in that view. It could be some specific page, several listings of other templates or just about anything you wish (see [**Listing Pages**](./listing-pages.html)).
</p>

Couch further supports two special URLs that are meant to list pages belonging to a partcular folder or to a particular time period.<br/>
Thus the _list view_ can be further categorised into two final views -

**1\. Folder view** - while listing pages belonging to a particular folder -<br/>
_<https://www.mysite.com/blog.php?f=3>_ or<br/>
*<https://www.mysite.com/blog/some_subfolder/>* (if pretty-urls activated)

**2\. Archive view** - while listing pages belonging to a particular time period -<br/>
_<https://www.mysite.com/blog.php?d=201005>_ or<br/>
_<https://www.mysite.com/blog/2010/05/>_ (if pretty-urls activated)

<p class="notice">The 'Folder view' and 'Archive view' URLs shown can be automatically generated by using the [__*folders*__](../tags-reference/folders.html) and [__*archives*__](../tags-reference/archives.html) tags.</p>

Once again, while accessing pages through these URLs, it is expected that you'll list pages belonging to the particular folder or the archive period. Couch sets variables that will help you in doing so, but you can choose to ignore everything and show whatever you desire.

### HOW TO RECOGNIZE A VIEW

The following variables are set by Couch for your script to know the view it is executing in -

**Page View**<br/>
k_is_page = '1'

**List View**<br/>
k_is_list = '1'<br/>
k_is_home = '1'

**Folder View**<br/>
k_is_list = '1'<br/>
k_is_folder = '1'

**Archive View**<br/>
k_is_list = '1'<br/>
k_is_archive = '1'<br/>
k_is_day = '1' (if daily archive)<br/>
k_is_month = '1' (if monthly archive)<br/>
k_is_year = '1' (if yearly archive)

Notice how the variable *k_is_home* gets set only when _blog.php_ gets executed in a simple _List view_ that is neither a _Folder view_ nor an _Archive view_. We can call this view - the **Home view**.

Taking the example of _blog.php_, notice how the views and variables change according to the URL used to access it -<br/>
_<https://www.mysite.com/blog/>_<br/>
k_is_list = 1<br/>
k_is_home = 1

*<https://www.mysite.com/blog/some_subfolder/>*<br/>
k_is_list = 1<br/>
k_is_folder = 1

_<https://www.mysite.com/blog/2010/05/>_<br/>
k_is_list = 1<br/>
k_is_archive = 1

*https://www.mysite.com/blog/some_page_name.html>*<br/>
k_is_page = 1

For clonable templates, you'll have to recognize which view the template is being executed in (by testing the variables given above) and then display the relevant data.<br/>
It could be something like the following -

```html
<cms:if k_is_page >
    <!-- Page view - display current page here -->
<cms:else />
    <!-- List view -->
    <cms:if k_is_folder >
        <!-- Folder view - display a list of pages belonging to this folder here -->
    </cms:if>

    <cms:if k_is_archive >
        <!-- Archive view - display a list of pages belonging to this time period here -->
    </cms:if>

    <cms:if k_is_home >
        <!-- Neither a folder view nor archive view - display a list of ALL pages cloned from this template here -->
    </cms:if>
</cms:if>
```

or

```html
<cms:if k_is_list >
    <!-- List view - display list of pages here -->
<cms:else />
    <!-- Page view - display current page here -->
</cms:if>
```

Please also see:

[**Variables available in Views**](./variables-in-views.html) for a comprehensive list of variables that become available during the different views and [**Listing Pages**](./listing-pages.html) for how to list pages in concordance to the views.
