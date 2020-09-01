# Perfil do Usuário

## Dados do usuário
- chamar: nome ou usarname/ url_foto do usuário (Tabela user criada pelo laravel) / utilizando nome da tabela
      
        {{ Auth::user()->username }} 
        {{ Auth::user()->url_foto }}

## Projetos do usuário 
#### apresentar projetos criados pelo usuário
- Entrar em: `App/Http/Controllers/homeController`
- no método de apresentação da view: `index()` ----- adicionar os Projetos filtrados pelo id do user
- Para que tenhamos acesso à tabela, chamamos o model referente ao projeto com seu caminho: 
              use App\Projeto;
              
- Para identificar o id do user logado, utilizaremos a rota `Auth`, que também deve ter ser caminho definido

              use Illuminate\Support\Facades\Auth;
              
- Além de filtrar os projetos pelo id, temos que guardá-los em uma variável para consulta futura.
          
                public function index()
                {
                    if (Auth::check()){
                        $projetos = Projeto::where('user_id', auth()->user()->id)->get();
                        return view('home', compact('projetos'));
                    }
                }

- Verificamos, então, se o usuário está logado `Auth::check()` e buscamos no model `Projeto` 
os projetos que tenham `user_id` (criadores) igual ao `id` do usuário logado. A variável `$projetos` guardará todos os resultados da busca
-  Retornamos a view e adicionamos `compact('projetos')` para que essa variável possa ser acessada na view.

### View
- Criamos vários cards com as informações acessadas no banco de dados
- Para que ocorra a repetição de criar um card de cada vez, utilizamos o `@foreach` e `@endforeach`. Ele considera que a variável `$projetos` é uma caixa que contém 
 vários projetos, cada um sinalizado como `$p`.
- Então, para cada `$p`, podemos acessar seus dados contidos nas colunas.

                    @foreach ( $projetos as $p )
                          <div class="col-md-4 clearfix d-none d-md-block ">
                              <div class="card mb-2 ">
                                  <img class="card-img-top " src="{{ $p->url_foto}}" alt="Card image cap ">
                                  <div class="card-body ">
                                      <h4 class="card-title ">{{ $p->titulo }}</h4>
                                      <p class="card-text ">{{ $p->descricao }}</p>
                                      <a class="btn btn-primary" href="/projeto/{{ $p->id }}">Button</a>
                                  </div>
                              </div>
                          </div>
                      @endforeach
                    
 - O link chamado `Button` cria uma botão que envia o usuário para a página do `Perfil do Projeto`. Ele direciona para a rota `"/projeto/{{ $p->id }}"` que será criada a seguir.
 
 - Entrar em: `routes/web.php`. Criar rota para perfil do Projeto especificado pelo id do projeto.
        
          Route::get('/projeto/{id}', 'PerfilProjeto@show');
 
 Então, assim que o user apertar no botão "Ver mais", ele enviará o id do projeto para a rota e será direcionado para o projeto específico.
 `@show` será um método de visualização da view adicionado ao controller `PerfilProjeto`.
 
 - Entrar em: `App/Http/Controllers/PerdilProjeto.php`
 - Adicionar função que retornará view especificada pelo **id do projeto**. Esse método deve receber o $id encaminhado pelo botão e pela rota e fazer a pesquisa
  para retornar apenas o projeto do id.
 
          public function show($id)
          {
            $projeto = Projeto::find($id);
            return view('perfil-projeto', compact('projeto'));
          }

      
