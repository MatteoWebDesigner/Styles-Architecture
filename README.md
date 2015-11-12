# CSS Architecture

## Principles:
* create patterns
* decoupling
* single responsibility
* separation of concerns
* encapsulation


## CSS Architecture base
CSS main architecture, style guide proposal:
http://cssguidelin.es/


## Code organisation ITCSS
```scss
* Setting    =>  $brand-color: red;                 // Config, theme setting, brand color
* Tools      =>  @mixin name-mixin () {}            // Global available tools like function, mixin (the original name was Tools)
* Generic    =>  * { box-sizing: border-box }       // Low specificity reset.css normalize.css
+ Vendor     =>  @import external stylesheet        // You cannot change the css here (It's not part of ITCSS)
* Base       =>  h1 { font-size: $font-size; }      // unclassed html
* Objects    =>  .o_ul-list {} .o_ul-list__item {}  // OOCSS, Design Pattern, No cosmetics, Agnostically named
+ Layout     =>  .l_side-body {}                    // Layout are essentially a subset of Object
* Components =>  .c_card {} .c_card__badge {}       // Designed pieces of UI
+ Shame      =>  .s_website-section-component {}    // Website section specific classes (It's not part of ITCSS)
* Trumps     =>  .t_text-center { !important; }     // Last resort, only affect one specific piece of DOM
[check](http://cssguidelin.es/#table-of-contents)
```


## CSS Naming convention
prefix + BEM
```sass
.{group}_{file-namespace-block}
```
## SASS variables naming convention
```sass
// setting.scss
$variable: 'value';                // global variable

// component.scss
$c_component__variable: 'value';   // public variable
.c_component {                     // block
    $variable: 'value';            // local variable
}
```

## Structure of CSS classes
vanilla CSS
```css
.c_component {}                     /* block */
.c_component--modifier {}           /* block modifier */
.c_component.is-active {}           /* block state */
.c_component__elment {}             /* element */
.c_component__elment--modifier {}   /* element modifier */
.c_component__elment.is-active {}   /* element state */

@media (min-width:730px; && max-width:1023px;) {}
@media (min-width:730px; && max-width:1023px;) {}
@media print {}
@supports ((prop: value)) {}
```

SCSS
```sass
// setting.scss
$variable: 'value';                // global variable

// component.scss
$c_component__variable: 'value';   // public variable
.c_component {                     // block
    $variable: 'value';            // local variable

    &--modifier {}                 // block modifier
    &.is-active {}                 // block state

    &__elment {                    // element
        &--modifier {}             // element modifier
        &.is-active {}             // element state
    }

    @media #{$screen__media_tablet} {
        $variable: 'value';  // local media variable
    }
    @media #{$screen__media_desktop} {}
    @media print {}
    @supports ((prop: value)) {}
}
```

## Type of CSS Abstractions
object      .grid
component   .ui-button
shame       .bof-component
trumps      .text-center

## Good Patterns
* classes composition can sometime overcomplicate the relationship between different styles. Follow this principle "duplication is better than the wrong abstraction" cit. Harry Roberts
* use preprocessor __Sass__ or __Less__
* use __SMACSS__ principles to organise and split your styles code
* mobile first
* use __BEM__ naming convention and include sass filename as part of the namespace { .c_filename__elm }
* flat selector { provide solid architecture and even better performance }
* classes should only concern within their element and they __should not interfere with the parent element__ { that's why box-sizing: border-box is great }
* classes should include __state classes__ private and public __variables__ and __medias__ definition { tablet, desktop, print }
* use __@extend__ and __silent classes__
* keep __inheritance__ with @extend at __one level down__
* use __@mixin__ when you want create a list of custom properties
* use @extend only if you are going to apply those css rules to the component sharing the same concern
* in your html and css __separete structure from skin__
* use index.scss to load dependencies
* code should be self documented
* order css rules alphebetically { minor improvment }
* create a single page example { showing every components, create awareness of your styles }
* When you use base utils classes like .padding-right should use a set of dimensions. Like $gutter-base: 5px; generate css .padding-right-{n} { size=> 5, 10, 15, 20 ... } and should use media query too.
* Work with designer is a key factor to create reusable design and reusable css, think in term of components instead of web page.
* use gulp __sourcemap__, _autoprefix_ and _minify_

## Bad Patterns
* you can style general elements (a p h1 ...) but you cannot rely on those selector because html elements purpose is to give meaningful content for the search engine. When you are applying a class to those element you should apply a reset upon those style to keep a more consiste and solid architecture: html=> p.c_text-description - css=> .c_text-description { reset and css styles }
* do not overuse gris classes, when the container is smaller than the mobile size is not worth it
* separate state.scss
* avoid id to styles and long selector
* share the same class or attribute
* css tight to the strtucture of the html ( .footer a.component ) are fragile
* css specificity are evil, make your architecture fragile, use ITCSS and a component base approch

## Note:
Good/bad pattern are applied to style concerns. You can use id, and state classes if used in javascript concern scenario.
Comparing SMACSS I removed states.

## Resources:
* [Video Organizing CSS with OOCSS, SMACSS, and BEM](https://www.youtube.com/watch?v=IKFq2cSbQ4Q)
* [Slides Organizing CSS with OOCSS, SMACSS, and BEM](https://speakerdeck.com/mattstauffer/organizing-css-oocss-smacss-and-bem)
* [Docs SMACSS](https://smacss.com/)
* [Article BEM name spacing conventions](http://csswizardry.com/2015/03/more-transparent-ui-code-with-namespaces/)
* [Slide ITCSS solve specificity problems](https://speakerdeck.com/dafed/managing-css-projects-with-itcss)
* [Docs CSS Guide line](http://cssguidelin.es/)
