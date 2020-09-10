## Dicas úteis

### Botão: voltar à págima anterior> `{{ URL::previus() }}`

      <a class="btn-deep-orange btn align-self-center" href="{{ URL::previous() }}" >Voltar</a>
      
### Confirmação para detelar algo
- No p´roprio botão de delete adicionar evento do javascript: `onclick="return confirm('Deseja mesmo deletar esse projeto?');"`. Aparecerá 
um alerta pedindo a confirmação.

      <button class="btn-orange btn p-1" onclick="return confirm('Deseja mesmo deletar esse projeto?');">Delete</button>
      
### Confirmar se mensagem foi enviada no formulário de contato
- No método onde foram coladas as ações para envio de mensagem, adicionar ao `return redirect('')`, que redirecionará para a página escolhida, um `with('mensagem', 'texto 
 da mensagem'). 
 
       public function enviaContato(Request $request)
      {
        Mail::to('torocultural@gmail.com')->send(new ContatoEmail($request));
        return redirect('/')
                ->with('mensagem', 'Mensagem enviada!');
      }
 
 
- Adicionar na view onde será exibida a mensagem de confirmação de envio a condição para a exibição da mensagem

      @if(session('mensagem'))
          <div class="alert alert-success">
              <p>{{session('mensagem')}}</p>
          </div>
      @endif

### Alterar formato da data que vem pelo database
- Vamor utilizar o mutate. Entrar no model referente a tabela. Exemplo: Projeto.
- Criar nova função para pegar a informação que vem do banco e atribuir nova caraterística a ela.
- Sempre colocar `get` + `nomedacoluna(nãoprecisacolocar o _ )` + `Attributte` como nome do método.

      public function getDataderealizacaoAttribute($value)
      {
        //ALETERAR FORMATO DA DATA QUE VEM DO DATABASE
        return \Carbon\Carbon::parse($value)->format('d/m/Y');
      }
