# CRUD

### Criar

- Cadastro de projetos com formulário

  Criar formulário com `method="POST"` e `action = "/projeto/create"`. Ele criará uma rota `post`com método para obtenção de dados dos inputs e enviou ao BD.
  
      <form method="POST" action = "/projeto/create"> 
      @csrf
      
      </form>
      
- Colocar após abertura do formulário o `@csrf` (Default)
- Criar rota para página de criação do projeto (página do formulário)
      
      Route::get('/projeto/create', 'PerfilProjeto@create');

- Criar rota para acessar, validadar e enviar os dados fornecidos pelo formulário

      Route::post('projeto/create', 'PerfilProjeto@store');
      
- Método no `PerfilProjetoController` para chamar a view de criação (formulário)

      public function create()
      {
          return view ('projetos.create');
      }
      
- Criar novo projeto para ser enviado ao banco de dados.

      public function store(Request $request)
      {
        $produto = new Produto; //Class Projeto
        $projeto->user_id = auth()->user()->id;    //Necessário enviar o dado sobre o usuario criador, que é o mesmo logado no sistema
        $produto->descricao= request('descricao'); // Envia para a coluna descricao do modelo Produto o conteudo do input de name = "descricao"
        $produto->titulo= request('titulo');       // Envia para a coluna titulo do modelo Produto o conteudo do input de name = "titulo"
        $produto->save();                          // Salva no banco de dados
        return redirect('/projeto/'.$projeto->id); //Ao enviar os dados, redirecionará para o perfil do projeto
      }
- Salvar imagem na tabela projetos. Na mesma função `store()`, adicionar validação para o banco de dados. 
        
  Verificar se um arquivo foi enviado e se ele é válido. Se sim, ele procurará na `$request` o conteúdo no input `url_foto` e salvará 
   com `->store('projetos')`. Essa função `store` criará a pasta **projetos** que será guardada em: `storage\app\ `. Já guardará o nome criptografado da imagem. 
   Todo esse processo guardará uma url da imagem da variável `$url_foto` e assim, é possível associá-la ao objeto `$projeto` criado.
  
      public function store(Request $request)
      {
            $projeto = new Projeto;
            if($request->hasfile('url_foto') && $request->url_foto->isvalid()){
                $url_foto = $request->url_foto->store('projetos');
            } 
            $projeto->url_foto = $url_foto;
          //RESTANTE DO CONTEUDO JÁ MOSTRADO A CIMA
       }

      
      
### EDITAR

- Copiar o arquivo `create` como `edit.blade.php`. A estrutura do formulário será igual, tirando o método.
- Como podemos observar, o formulário ainda recebe o method `post`, porque alguns navegadores não identificam o method `put` ainda.
  O method put é o utilizado para criar edições. Para que o laravel entenda que se trata de uma edição. Colocamos o @melhod('put') logo abaixo da abertura da tag`<form>`.
  Assim, não dará erro no navegador e nem na requisição.
  
      <form method="POST" action = "projeto/edit/{{ $projeto->id }}"> 
      @method('put)
      @csrf
              //inputs e labels
      </form>
      
- Ao acessar a página do projeto, teremos um botão editar que terá um `href = '/projeto/edit/{{ $projeto-> id}}'`. Assim, logo que apertarmos o botão, a página eviará o id do 
projeto em questão para a rota que será criada a seguir: 
      
- Criar rota para exibição da view com formulário para edição. 

      Route::get('/projeto/edit/{id}', 'PerfilProjeto@edit');
      
- Agora é necessário criar o método `edit` para retornar o formulário de edição.

      public function edit($id)
      {
        $projeto = Projeto::find($id); //Como ele receberá um id, poderá procurar na tabela projetos todas as informações referentes a ele e enviar para a view de edição

        return view('projetos.edit', compact('projeto'));
      }
      
 - Vamos entrar com esses dados recuperados dentro do formulário. Adicionando o `value` nos inputs.
 
       <form method="POST" action = "projeto/edit/{{ $projeto->id }}"> 
            @method('put)
            @csrf
                    <input type="text" class="form-control" name="titulo" id="titulo" placeholder="Nome" value="{{ $produto->titulo }}">
       </form>
 - Assim que o formulário for carregado trará as informações gravadas no banco de dados. Se alguma alteração for realizada, será necessário atualizar o banco.
 - a `action` pode mostrar a mesma rota, o que diferenciará é o tipo de solicitação. Agora usamos o `put`.
 
- Criar rota para acessar, validadar e enviar os dados fornecidos pelo formulário. Essa rota só será utilizada quando o botão de enviar do formulário for acessado.

      Route::put('/projeto/edit/{id}', 'PerfilProjeto@update');
- Criar método `update` no controller do Perfil do projeto

      public function update($id)
      {
        
        $projeto = Projeto::find($id);         //Encontrar o projeto

        //Adicionar os valores do formulário de edição ao $projeto
        $produto->descricao= request('descricao'); 
        $produto->titulo= request('titulo');      

        $produto->save();                       //salvar novamente
        return redirect('/projeto/'.$projeto->id);
      }

### Deletar

- Criar botão `delete` no perfil do projeto com o esquema de manter o `method= "POST"` e adicionar o `@method('delete')`

      <form action="/projeto/delete/{{$projeto->id}}" method="POST">
            @method('delete')
            @csrf
            <button type="submit" class="btn badge badge-danger">Deletar</button>
      </form>

- Criar rota do delete

      Route::delete('/projeto/delete/{id}', 'PerfilProjetoController@delete');
      
- Criar método `delete` no controller

       public function delete($id)
       {
        Projeto::where('id', $id)->delete();
        return redirect('/home');
       }
