# TYPO3 v10.4 Multi-Level Menu
TYPO3 v10.4 Multi-Level Menu with MenuProzessor, Bootstrap v4.5, SmartMenus v1.1
## /Configuration/TypoScript/setup.typoscript
Fluidtemplate with MenuProzessor
```
page = PAGE
page {
    10 = FLUIDTEMPLATE
    10 {
            dataProcessing {
            10 = TYPO3\CMS\Frontend\DataProcessing\MenuProcessor
            10 {
                levels = 3
                includeSpacer = 1
                as = mainnavigation
            }
           }   
       }
    }
```
## /Resources/Private/Layouts/Page/Default.html
Layout with Main Menu
```
<html xmlns:f="http://typo3.org/ns/TYPO3/CMS/Fluid/ViewHelpers" data-namespace-typo3-fluid="true">
<f:spaceless>
<div id="top"></div>
<div class="body-bg">

    <a class="sr-only sr-only-focusable" href="#page-content">
        <span>{f:translate(key: 'skiptomaincontent', extensionName: 'theme')}</span>
    </a>

    <header id="page-header" class="">
        <f:render partial="Navigation/Main" arguments="{_all}" />
    </header>

    <div id="page-content" class="">
        <!--TYPO3SEARCH_begin-->
        <f:render section="Main" />
        <!--TYPO3SEARCH_end-->
    </div>

    <footer id="page-footer" class="">
    	<f:render section="Footer" />
    </footer>

</div>
</f:spaceless>
</html>
```
## /Resources/Private/Partials/Page/Navigation/Main.html
Main Navigation with Partials for Subtitle and Submenu
```
<nav class="navbar navbar-expand-md navbar-dark bg-dark fixed-top">
    <f:link.page pageUid="{rootPage}" class="navbar-brand">{siteTitle} Test</f:link.page>
    <button  class="navbar-toggler"
             type="button"
             data-toggle="collapse"
             data-target="#navbarNavDropdown"
             aria-controls="navbarNavDropdown"
             aria-expanded="false"
             aria-label="Toggle navigation"
    >
        <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNavDropdown">
        <ul class="nav navbar-nav mr-auto">
            <f:for each="{mainnavigation}" as="mainnavigationItem">
                <li class="nav-item{f:if(condition: mainnavigationItem.active, then: ' active')}{f:if(condition: mainnavigationItem.children, then:' dropdown')}">
                    <a class="nav-link"
                        href="{mainnavigationItem.link}"
                        target="{mainnavigationItem.target}"
                        <f:render partial="Navigation/SubTitle" arguments="{menuSubtitle: mainnavigationItem}"/>
                    >
                        {mainnavigationItem.title}
                    </a>
                    <f:if condition="{mainnavigationItem.children}">
                        <f:render partial="Navigation/MainSub" arguments="{mainsubmenu: mainnavigationItem.children}"/>
                    </f:if>
                </li>
            </f:for>
        </ul>
    </div>
</nav>
```
## /Resources/Private/Partials/Page/Navigation/MainSub.html
Sub menu
```
<ul class="dropdown-menu" aria-labelledby="nav-item-{mainnavigationItem.data.uid}">
    <f:for each="{mainsubmenu}" as="subItem">
        <f:if condition="{subItem.spacer}">
            <f:then>
                <li class="dropdown-divider"></li>
            </f:then>
            <f:else>
                <li {f:if(condition: subItem.children, then: ' class="dropdown"')}>
                    <a class="dropdown-item{f:if(condition: subItem.active, then:' active')}"
                        href="{subItem.link}"
                        <f:render partial="Navigation/SubTitle" arguments="{menuSubtitle: subItem}"/>
                        {f:if(condition: subItem.target, then: ' target="{subItem.target}"')}>
                        <span class="dropdown-text">{subItem.title}
                        <f:if condition="{subItem.current}"> <span class="sr-only">(current)</span></f:if>
                        </span>
                    </a>
                    <f:if condition="{subItem.children}">
                        <f:render partial="Navigation/MainSub" arguments="{mainsubmenu: subItem.children}"/>
                    </f:if>
                </li>
            </f:else>
        </f:if>
    </f:for>
</ul>
```
## /Resources/Private/Partials/Page/Navigation/SubTitle.html
Set subtitle for title tag if available
```
<f:if condition="{menuSubtitle.data.subtitle}">
<f:then>
    title="{menuSubtitle.data.subtitle}"
</f:then>
<f:else>
    title="{menuSubtitle.title}"
</f:else>
</f:if>
```
