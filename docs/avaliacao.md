# Avaliação — Engenharia de Software
**Sistema Integrado de Gestão de Farmácia — MVP Definido pelo Estudante**

Aluno: Felipe dos Santos Larangeiras
RA: 24000669
Data: 26/03/2026

---

# 1. Definição do MVP
> Meu MVP cobre o processo de venda de produtos na farmácia, desde a identificaão ou cadastro do cliente até a finalização da venda e emissão do comprovante

O que está **dentro** do MVP
  -Identificação/cadastro de cliente
  -Consulta de produtos
  -Verificação de estoque
  -Registro de venda
  -Atualização automatica de estoque
  -Emissão de comprovante
  -Geração de contas a receber
  
O que está **fora** do MVP  
  -Gestão completa de fornecedores
  -Controle detalhado de contas a pagar
  -Relatorios avançados
  -Transferências entre unidades
  -Administração completa de usuarios

Por que você fez essas escolhas  
  -O foco foi definido no processo de venda por ser a principal operação da farmacia, garantindo valor imediato ao negocio e permitindo validar rapidamente o sistema em uso real.

---

# 2. Regras de Negócio (mínimo: 5)
Liste e descreva **cada RN** de forma clara.

**RN01 —**  Produtos só podem ser vendidos se houver quantidades disponiveis em estoque.
**RN02 —**  Toda venda realizada deve atualizar automaticamente o estoque.
**RN03 —**  Vendas a prazo devem gerar automaticamente um registro em contas ao receber.
**RN04 —**  Compras de fornecedores devem gerar automaticamente um registro em contas a pagar.
**RN05 —**  Cada unidade deve ter um controle de estoque indenpendente.
**RN06 —**  Produtos abaixo do estoque minimo deve gerar um alerta para o gerente.
**RN07 —**  Apenas farmacêuticos devem autorizar a venda de remedios controlados.
**RN08 —**  Clientes podem ser cadastrado no momento da venda.

---

# 3. Requisitos Funcionais (mínimo: 8)
Liste os requisitos funcionais do seu MVP.

**RF01 —**  Cadastrar, editar e consultar produtos.
**RF02 —**  Registrar vendas.
**RF03 —**  Consultar estoque por unidade.
**RF04 —**  Atualizar estoque automaticamente após movimentação.
**RF05 —**  Cadastrar clientes.
**RF06 —**  Registrar compras de fornecedores.
**RF07 —**  Gerar contas a pagar automaticamente.
**RF08 —**  Gerar contas a receber automaticamente.
**RF09 —**  Emitir comprovante de vendas.
**RF10 —**  Gerar relatorios gerenciais.

---

# 🛡 4. Requisitos Não Funcionais (mínimo: 4)
Liste os RNFs do sistema conforme seu MVP.

**RNF01 —**  O sistema deve garantir integridade dos dados em tempo real.
**RNF02 —**  O sistema deve ter o controle de acesso baseado em perfis de usuarios.
**RNF03 —**  O sistema deve ter alta disponibilidade.
**RNF04 —**  O sistema de registrar logs de operação.
**RNF05 —**  O sistema deve ter uma interface intuitiva e facil de uso.

---

# 5. Casos de Uso (mínimo: 10)
### Inserir **diagrama de casos de uso geral**, demonstrando claramente:

Atendente -> (Realizar Vendas)
Atendente -> (Cadastrar Clientes)

Realizar Vendas -> <include> (Identificar Clientes)
Realizar Vendas -> <include> (Consultar Produtos)
Realizar Vendas -> <include> (Verificar Estoque)

(Registrar Venda a Prazo) -> <extend> (Realizar Venda)
(Emitir Comprovante) -> <extend> (Finalizar Venda)
(Validar Receita) -> <extend> (Realizar Venda)

Farmacêutico - (Validar Receita)

---

# 6. Documentação dos Casos de Uso
Para **cada caso de uso**, utilize o template abaixo:

---

## **UC01 — Realizar Vendas**
**Ator(es): Atendente**  
**Descrição: Permite registrar a venda de produtos ao cliente.**  
**Pré-condições: Sistema ativo; produtos cadastrados.**  
**Pós-condições: Venda registrada e estoque atualizado.**  

### Fluxo Principal
1.  Atendente inicia venda
2.  Identifica cliente
3.  Consulta produto
4.  Verifica estoque
5.  Informa quantidade
6.  Sistema calcula total
7.  Finaliza venda

### Fluxos Alternativos / Exceções
- FA01 —  Produto sem estoque → sistema bloqueia venda
- FA02 —  Cliente não cadastrado → permite cadastro

### Relacionamentos
- **Include: Identificar Cliente, Consultar Produto, Verificar Estoque** 
- **Extend: Registrar Venda a Prazo, Validar Receita**  

### Inserir o diagrama de atividades do Caso de Uso, demonstrando tudo o fluxo princial e alternativos/exceções.

@startuml 
start 
:Identificar cliente;
:Consultar produto;
:Verificar estoque;

if (Produto disponível?) then (Sim) 
  :Informar quantidade; 
  :Calcular total; 

if (Pagamento a prazo?) then (Sim) 
  :Gerar conta a receber; 
else (Não) 
  :Pagamento à vista;
endif 

:Finalizar venda; 
:Emitir comprovante; 
else (Não) 
  :Exibir erro de estoque; 
endif 

stop 
@enduml

-------------------------------------------------------------------------------

## **UC02 — Cadastrar Cliente**
**Ator(es): Atendente**  
**Descrição: Permite cadastrar um novo cliente.**  
**Pré-condições: Sistema ativo.*  
**Pós-condições: Cliente cadastrado.**  

### Fluxo Principal
1.  Inserir dados do cliente
2.  Validar dados
3.  Salvar cliente

### Fluxos Alternativos / Exceções
- FA01 —  Dados Invalidos → sistema solicita correção


### Relacionamentos
- **Include: Registrar Cliente** 
- **Extend: Validar Registro**
- Obs: Pode ser acionado como fluxo alternativo de UC01.

### Inserir o diagrama de atividades do Caso de Uso, demonstrando tudo o fluxo princial e alternativos/exceções.

@startuml 
start 

:Inserir dados do cliente; 
:Validar dados; 

if (Dados válidos?) then (Sim) 
  :Salvar cliente; 
else (Não) 
  :Exibir erro; 
endif 

stop 
@enduml

-------------------------------------------------------------------------------

## **UC03 — Consultar Produtos**
**Ator(es): Atendente**  
**Descrição: Permite buscar produtos cadastrados.**  
**Pré-condições: Produtos cadastrados.**  
**Pós-condições: Produto exibido.**  

### Fluxo Principal
1.  Informar nome ou código
2.  Sistema busca produto
3.  Exibe resultado

### Fluxos Alternativos / Exceções
- FA01 —  Produto não encontrado.

### Relacionamentos
- **Include: Identificar Cliente, Consultar Produto, Verificar Estoque** 
- **Extend: Registrar Venda a Prazo, Validar Receita**
- obs:É incluído por UC01

### Inserir o diagrama de atividades do Caso de Uso, demonstrando tudo o fluxo princial e alternativos/exceções.

@startuml 
start 
:Informar nome ou código; 
:Buscar produto; 

if (Produto encontrado?) then (Sim) 
  :Exibir produto; 
else (Não)   
  :Exibir mensagem de erro;   
endif 

stop 
@enduml

-------------------------------------------------------------------------------

## **UC04 — Verificar Estoque**
**Ator(es): Sistema**  
**Descrição: Verifica a quantidade disponível do produto.**  
**Pré-condições: Produto selecionado.**  
**Pós-condições: Quantidade exibida.**  

### Fluxo Principal
1.  Consultar estoque
2.  Exibir quantidade

### Fluxos Alternativos / Exceções
- FA01 —  Produto sem estoque

### Relacionamentos
- **Include: ** 
- **Extend: **  

### Inserir o diagrama de atividades do Caso de Uso, demonstrando tudo o fluxo princial e alternativos/exceções.

@startuml 
start 

:Consultar estoque; 
if (Quantidade disponível?) then (Sim) 
  :Exibir quantidade; 
else (Não) 
  :Informar indisponível; 
endif 

stop 
@enduml

-------------------------------------------------------------------------------

## **UC05 — Finalizar Venda**
**Ator(es): Atendente**  
**Descrição: Conclui a venda no sistema.**  
**Pré-condições: Venda iniciada.**  
**Pós-condições: Venda concluída.**  

### Fluxo Principal
1.  Confirmar pagamento
2.  Registrar venda
3.  Atualizar estoque

### Fluxos Alternativos / Exceções
- FA01 —  Falha no pagamento

### Relacionamentos
- **Include: ** 
- **Extend: Emitir Comprovante**
- obs:Parte final do UC01

### Inserir o diagrama de atividades do Caso de Uso, demonstrando tudo o fluxo princial e alternativos/exceções.

@startuml 
start 

:Confirmar pagamento; 
:Registrar venda; 
:Atualizar estoque; 

stop @enduml

-------------------------------------------------------------------------------

## **UC06 — Registrar Venda a Prazo**
**Ator(es): Atendente**  
**Descrição: Permite registrar venda com pagamento futuro.**  
**Pré-condições: Venda em andamento.**  
**Pós-condições: Conta a receber gerada.**  

### Fluxo Principal
1.  Selecionar venda a prazo
2.  Informar dados
3.  Gerar conta a receber

### Fluxos Alternativos / Exceções
- FA01 —  Dados inválidos

### Relacionamentos
- **Include: Gerar conta a receber** 
- **Extend: Realizar Venda**

### Inserir o diagrama de atividades do Caso de Uso, demonstrando tudo o fluxo princial e alternativos/exceções.

@startuml 
start 

:Selecionar a prazo; 
:Gerar conta; 

stop 
@enduml

-------------------------------------------------------------------------------

## **UC07 — Emitir Comprovante**
**Ator(es): Sistema**  
**Descrição: Gera comprovante da venda.**  
**Pré-condições: Venda finalizada.**  
**Pós-condições: Comprovante emitido.**  

### Fluxo Principal
1.  Gerar comprovante
2.  Exibir ou imprimir
    
### Fluxos Alternativos / Exceções
- FA01 —  Falha na geração

### Relacionamentos
- **Include: ** 
- **Extend: Finalizar Venda**

### Inserir o diagrama de atividades do Caso de Uso, demonstrando tudo o fluxo princial e alternativos/exceções.

@startuml 
start 

:Gerar comprovante; 
:Exibir; 

stop 
@enduml

-------------------------------------------------------------------------------

## **UC08 — Validar Receita**
**Ator(es): Farmacêutico**  
**Descrição: Autoriza venda de medicamentos controlados.**  
**Pré-condições: Receita apresentada.**  
**Pós-condições: Venda autorizada ou negada.**  

### Fluxo Principal
1.  Receber receita
2.  Validar receita
3.  Autorizar venda

### Fluxos Alternativos / Exceções
- FA01 —  Receita inválida

### Relacionamentos
- **Include: ** 
- **Extend: Realizar venda**

### Inserir o diagrama de atividades do Caso de Uso, demonstrando tudo o fluxo princial e alternativos/exceções.

@startuml 
start 

:Validar receita; 
if (Válida?) then (Sim) 
  :Autorizar; 
else :Negar; 
endif 

stop 
@enduml

-------------------------------------------------------------------------------

## **UC09 — Atualizar Estoque**
**Ator(es): Sistema**  
**Descrição: Atualiza o estoque após movimentações.**  
**Pré-condições: Movimentação registrada.**  
**Pós-condições: Estoque atualizado.**  

### Fluxo Principal
1.  Receber movimentação
2.  Atualizar quantidade
3.  Salvar

### Fluxos Alternativos / Exceções
- FA01 —  Erro de atualização

### Relacionamentos
- **Include: ** 
- **Extend: Realizar venda**
- obs: Executado internamente após UC01 e UC05

### Inserir o diagrama de atividades do Caso de Uso, demonstrando tudo o fluxo princial e alternativos/exceções.

@startuml
start

:Atualizar estoque;
:Salvar;

stop
@enduml

-------------------------------------------------------------------------------

## **UC10 — Gerar Conta a Receber**
**Ator(es): Sistema**  
**Descrição: Cria registro financeiro da venda a prazo.**  
**Pré-condições: Venda a prazo realizada.**  
**Pós-condições: Conta registrada.**  

### Fluxo Principal
1.  Criar lançamento
2.  Definir valor
3.  Definir vencimento
4.  Salvar

### Fluxos Alternativos / Exceções
- FA01 —  Falha no registro

### Relacionamentos
- **Include: ** 
- **Extend: **
- obs:Incluído por UC06


### Inserir o diagrama de atividades do Caso de Uso, demonstrando tudo o fluxo princial e alternativos/exceções.

@startuml 
start 

:Criar conta; 
:Salvar; 

stop
@endum

-------------------------------------------------------------------------------

Diagrama de Atividade Geral


@startuml
start

:Iniciar atendimento;

:Identificar cliente;
if (Cliente cadastrado?) then (Sim)
else (Não)
  :Cadastrar cliente;
endif

:Consultar produto;
if (Produto encontrado?) then (Sim)
else (Não)
  :Exibir erro produto não encontrado;
  stop
endif

:Verificar estoque;
if (Produto disponível?) then (Sim)
else (Não)
  :Exibir erro de estoque;
  stop
endif

:Informar quantidade;
:Calcular total;

if (Medicamento controlado?) then (Sim)
  :Validar receita;
  if (Receita válida?) then (Sim)
  else (Não)
    :Negar venda;
    stop
  endif
endif

if (Pagamento a prazo?) then (Sim)
  :Registrar venda a prazo;
  :Gerar conta a receber;
else (Não)
  :Pagamento à vista;
endif

:Finalizar venda;

:Atualizar estoque;

:Emitir comprovante;

stop
@enduml


-------

(Não conseguiu fazer em imagem)
