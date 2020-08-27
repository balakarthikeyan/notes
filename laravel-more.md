## CDN for Bootstrap & Jquery
```
<link href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/4.4.1/css/bootstrap.min.css" rel="stylesheet" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validate/1.19.0/jquery.validate.js"></script>  
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validate/1.19.0/additional-methods.min.js"></script>  
```
## Mixins
#### Run all Mix
`npm run dev`

#### Run all Mix and minify
`npm run production`

#### Watching Assets For Changes
`npm run watch`

#### Loading Js & Css
```
mix.sass('resources/sass/app.sass', 'public/css')
mix.styles([
    'public/css/reset.css',
    'public/css/common.css'
], 'public/css/app.css');

mix.js('resources/js/app.js', 'public/js');
mix.js('resources/js/app.js', 'public/js').version();  
mix.scripts([
    'public/js/utils.js',
    'public/js/theme.js'
], 'public/js/all.js');

mix.copy('resources/font-awesome/fonts/*', 'public/fonts/');
mix.copyDirectory('resources/img', 'public/images');
```
## Blade directives:

```
@include pulls in information from the files in the includes directory.
@yield pulls in information from the content section of the home page.
@extends tells Laravel that the contents of this file extend another view. It also defines the view that it's extending as our default template: resources/views/layouts/default.blade.php.
@section defines the beginning of a section, which we have named content. Everything contained in this section will appear in the @yield which we defined in the template.
@endsection closes that section. 
@section and @endsection are terms which must be used in pairs, much like other HTML tags (for example, <div> and </div>).
```
