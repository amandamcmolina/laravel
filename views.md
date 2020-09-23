## Importando views para projeto Laravel
- **Exemplo: Tenho um html pronto chamado index.html e passarei para o laravel**

- Entrar na pasta `resources` / `views` / copiar html dentro da pasta.
- Alterar extensão `.html` para `index.blade.php` (desse modo, poderemos usar a linguagem blade que facilitará o acesso ao php dentro da view).
- Se o projeto tiver muitas páginas e nelas, o cabeçalho e o footer, por exemplo, se repetirem, podemos criar um layout base. 
- Criar pasta na `views` chamada `layouts` e adicionar um arquivo chamado `layoutBase.blade.php`. 
- No `layoutBase` se adiciona todo a parte de **html** que será repetida, e onde não for repetido, como no `main` da página adicionar, por exepmlo, `@yield('conteudo')`.
Se as páginas têm css diferentes, podemos adicionar outro **@yield('')**, como `@yield('css')`. Sempre com um nome claro.
 
        <!doctype html>
        <html>
          <head>
            ....
            @yield('css')
            
          </head>
          <body>
            <header>
              ....
            </header>
            
            @yield('conteudo')
            
            <footer>
              ....
            </footer>
          </body>
        </html>
        
- Já nas páginas que realmente serão exibidas, como a `index.blade.html`, chamamos o layout base da seguinte maneira: `@extends('pasta.arquivo')`. 
No caso: `@extends('layouts.layoutBase')`.
- E para chamar os **@yields**, utilizamos `@section('nomeDaYield')`conteudo com tag `@endsection`

          @sextends('layouts.layoutBase')
          
          @section('css')
                <link rel="stylesheet" href="{{ asset('css/style.css') }}">
          @endsection


- Se a view adionada fizer parte de um crud, por exemplo, criar pasta especial nas views para este crud, como: `usuarios`, `projetos`. Tudo o que é referente a ele entrará aí:  Create, edit, update

- Todos os arquivos **Javascript** **css** e **imagens estáticas** serão adionados à pasta: **public**. Então, apenar vamos copiar dentro da pasta.


#### Atenção ao utilizar links e imagens dentro da view
- Fazer as seguintes alterações:

**img**  ->  `src="img/filmeUm.jpg"`  substituir por: `src="{{ url('img/filmeUm.jpg') }}"`
**links no head**  ->  `href="css/style.css` substituir por: `href="{{ asset('css/style.css') }}"`

