## Blade directives:

```
@include pulls in information from the files in the includes directory.
@yield pulls in information from the content section of the home page.
@extends tells Laravel that the contents of this file extend another view. It also defines the view that it's extending as our default template: resources/views/layouts/default.blade.php.
@section defines the beginning of a section, which we have named content. Everything contained in this section will appear in the @yield which we defined in the template.
@endsection closes that section. 
@section and @endsection are terms which must be used in pairs, much like other HTML tags (for example, <div> and </div>).
```
