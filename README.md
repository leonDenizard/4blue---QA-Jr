# Relatório de Teste – QA

## Contexto
Este relatório apresenta os bugs identificados durante a exploração do sistema fornecido para o teste técnico.

Durante a análise foram avaliados:

- fluxo de autenticação  
- validação de formulário  
- experiência do usuário  
- segurança básica  

Foram identificados **8 problemas**, classificados por severidade e prioridade.

---

# Bugs de Segurança

## Título
Exposição de senha no console e armazenamento em LocalStorage após criação de conta

### Descrição
Durante a criação de conta foi identificado um problema de segurança: a senha informada pelo usuário fica exposta no **console do navegador** após o envio do formulário.

Além disso, a senha também é armazenada em **LocalStorage em texto legível**, permitindo que qualquer pessoa com acesso ao navegador visualize essa informação sensível.

### Passos para reproduzir
1. Acessar: https://qa-play-sim.lovable.app/criar-conta  
2. Preencher o formulário  
3. Submeter o cadastro  
4. Abrir o console do navegador (F12)  
5. Verificar os logs  
6. Acessar **Application → LocalStorage**

JAM do problema:  
https://jam.dev/c/73df2a5f-1149-410d-ac9e-a1de800bf18d

### Resultado atual
- A senha aparece no console  
- A senha é armazenada em LocalStorage em texto puro

### Resultado esperado
- Senhas não devem aparecer em logs do console  
- Credenciais sensíveis não devem ser armazenadas em LocalStorage em texto puro  
- Caso necessário, utilizar tokens ou armazenamento seguro

### Severidade
Crítico

### Prioridade
Alta

---

# Bugs Funcionais

## Título
Falha de validação nos campos do formulário de criação de conta

### Descrição
A tela de criação de conta não possui validação adequada nos campos do formulário. Os inputs aceitam valores inválidos que deveriam ser bloqueados ou validados antes do envio. O formulário também permite submissão sem dados.

Isso pode resultar em dados inconsistentes sendo registrados no sistema e comprometer a qualidade das informações armazenadas.

### Passos para reproduzir
1. Acessar: https://qa-play-sim.lovable.app/criar-conta  
2. Preencher os campos com dados inválidos:
   - Nome: `123456`
   - Telefone: `abcde`
   - E-mail: `teste123`
   - Senha: senha curta ou sem critérios mínimos
   - Confirmar senha: valor diferente da senha
3. Submeter o formulário  
4. Observar o comportamento do sistema

JAM do problema:  
https://jam.dev/c/6009fa1b-96e5-44d0-8a11-d9f7a996e654

### Resultado atual
- Nome aceita números  
- Telefone aceita letras  
- E-mail aceita qualquer texto  
- Senha não exige critérios mínimos  
- Confirmar Senha aceita valores diferentes da senha  
- Formulário permite envio com dados inválidos  
- Formulário permite envio vazio

### Resultado esperado
- Nome deve aceitar apenas caracteres válidos  
- Telefone deve aceitar apenas números ou formato válido  
- E-mail deve validar formato padrão  
- Senha deve possuir critérios mínimos de segurança  
- Confirmar Senha deve corresponder à senha  
- O formulário deve impedir envio com dados inválidos  
- O formulário deve impedir envio sem campos preenchidos

### Severidade
Alto

### Prioridade
Alta

---

## Título
Notificação de erro inesperado exibida após login bem-sucedido

### Descrição
Após realizar login com sucesso, o sistema exibe uma notificação de **“erro inesperado”**, mesmo com autenticação aparentemente funcionando.

Além disso, não existem logs no console nem requisições na aba **Network** que ajudem a identificar a causa do problema.

### Passos para reproduzir
1. Acessar: https://qa-play-sim.lovable.app/  
2. Inserir credenciais válidas  
3. Submeter login  
4. Observar o comportamento após autenticação

JAM do problema:  
https://jam.dev/c/0791cf2e-7ee1-478b-9d22-6b4cede12f97

### Resultado atual
- Login aparentemente realizado com sucesso  
- Sistema exibe notificação de **erro inesperado**  
- Não há logs ou requisições que expliquem o erro

### Resultado esperado
- Após login bem-sucedido não devem aparecer mensagens de erro  
- Caso ocorra erro, o sistema deve fornecer mensagens claras  
- Devem existir logs ou informações que permitam diagnóstico

### Severidade
Alto

### Prioridade
Alta

---

# Bugs de Fluxo

## Título
Inconsistência de fluxo após criação de conta na tela de sucesso

### Descrição
Após criar a conta, o sistema informa que o cadastro foi realizado com sucesso, mas a única ação disponível é **“Sair da conta”**.

Isso gera inconsistência, pois não está claro se o usuário já está autenticado.

### Passos para reproduzir
1. Acessar: https://qa-play-sim.lovable.app/criar-conta  
2. Preencher o formulário  
3. Submeter o cadastro  
4. Aguardar tela de sucesso

### Resultado atual
- Mensagem de sucesso exibida  
- Única ação disponível: **Sair da conta**

### Resultado esperado
O fluxo deveria seguir um padrão consistente, por exemplo:

- usuário é autenticado automaticamente após cadastro  
ou  
- usuário é redirecionado para a tela de login

### Severidade
Médio

### Prioridade
Média

---

# Bugs de UX / Interface

## Título
Inconsistência visual: sobreposição de campos e padronização incorreta de tipografia nos labels da tela de criação de conta

### Descrição
Na tela de criação de conta foi identificado um problema de layout onde os campos **Telefone** e **Confirmar Senha** apresentam sobreposição visual com outros inputs da interface.

Além disso, os labels dos campos estão em **caixa alta**, o que cria inconsistência visual com boas práticas de tipografia para formulários. Considerando o peso tipográfico utilizado, o padrão mais adequado seria **CamelCase** ou **Sentence case**, melhorando legibilidade e consistência da interface.

### Passos para reproduzir
1. Acessar o sistema: https://qa-play-sim.lovable.app/criar-conta  
2. Localizar os campos **Telefone** e **Confirmar Senha**  
3. Observar o posicionamento dos inputs e o estilo dos labels

### Resultado atual
- Os campos **Telefone** e **Confirmar Senha** apresentam sobreposição ou desalinhamento em relação aos outros inputs  
- Os labels estão em **UPPERCASE**, reduzindo a legibilidade

### Resultado esperado
- Todos os campos devem respeitar o espaçamento e alinhamento padrão do formulário  
- Os labels devem seguir um padrão tipográfico consistente (ex: CamelCase ou Sentence case)

### Severidade
Baixo

### Prioridade
Baixa

---

## Título
Formulário não apresenta mensagens de erro ou feedback visual

### Descrição
Ao submeter o formulário com dados inválidos ou incompletos, o sistema não apresenta mensagens de erro claras indicando qual campo precisa ser corrigido.

Isso prejudica a experiência do usuário, pois não há orientação para correção.

### Passos para reproduzir
1. Acessar a tela de criação de conta  
2. Preencher campos incorretamente ou deixar campos vazios  
3. Submeter o formulário

### Resultado atual
O sistema permite submissão ou não apresenta mensagens claras de erro.

### Resultado esperado
O sistema deve:
- Exibir mensagens de erro específicas por campo  
- Destacar visualmente campos inválidos  
- Orientar o usuário sobre como corrigir os dados

### Severidade
Médio

### Prioridade
Média

---

## Título
Campo de senha não possui opção de visualizar senha digitada

### Descrição
O campo de senha não apresenta opção de **mostrar/ocultar senha**, funcionalidade comum em formulários modernos que reduz erros de digitação.

### Passos para reproduzir
1. Acessar: https://qa-play-sim.lovable.app/criar-conta  
2. Localizar o campo de senha  
3. Verificar se existe opção de visualizar a senha

### Resultado atual
O campo de senha não possui opção de visualizar o conteúdo digitado.

### Resultado esperado
Deveria existir um botão ou ícone que permita alternar entre **mostrar** e **ocultar** a senha.

### Severidade
Baixo

### Prioridade
Baixa

---

## Título
Animação de hover no botão “Criar Conta” causa deslocamento visual desnecessário

### Descrição
O botão **Criar Conta** apresenta uma animação de hover que provoca um leve deslocamento para cima.

Esse comportamento pode causar sensação de instabilidade visual e não agrega valor à experiência do usuário em formulários.

### Passos para reproduzir
1. Acessar: https://qa-play-sim.lovable.app/criar-conta  
2. Localizar o botão **Criar Conta**  
3. Passar o mouse sobre o botão  
4. Observar a animação

### Resultado atual
O botão se desloca levemente para cima ao passar o mouse.

### Resultado esperado
O botão deve manter posição estável e utilizar feedback visual mais adequado, como:
- mudança de cor  
- alteração de sombra  
- leve destaque visual

### Severidade
Baixo

### Prioridade
Baixa

# Sugestões de melhorias

### 1. Melhorar segurança no tratamento de credenciais

Evitar que informações sensíveis, como senhas, sejam expostas no **console do navegador** ou armazenadas diretamente no **LocalStorage**.

Boas práticas recomendadas:

- Senhas nunca devem ser registradas em logs do console.
- Credenciais sensíveis não devem ser armazenadas no navegador em texto puro.
- Caso seja necessário persistir autenticação no cliente, utilizar **tokens de sessão emitidos pelo backend** (ex: tokens de autenticação) em vez de armazenar a senha do usuário.
- Garantir que dados sensíveis sejam tratados apenas pelo backend e armazenados de forma segura (ex: hash de senha).

Essas medidas reduzem significativamente riscos de **exposição de credenciais e comprometimento de contas de usuários**.

### 2. Melhorar feedback de validação dos formulários

Adicionar **mensagens de erro claras e específicas por campo**, informando exatamente o que precisa ser corrigido.

Exemplos:

- "Digite um e-mail válido"
- "A senha deve conter no mínimo 8 caracteres"

Isso melhora significativamente a **experiência do usuário** e reduz fricção no processo de cadastro.

---

### 3. Implementar validação em tempo real

Além da validação no momento do envio do formulário, seria interessante implementar **validação durante a digitação (real-time validation)**.

Dessa forma, o usuário recebe feedback imediato caso o valor inserido não esteja no formato esperado.

---

### 4. Melhorar consistência do fluxo de cadastro

Após a criação de conta, o sistema deveria seguir um fluxo claro e consistente, como por exemplo:

- **autenticar automaticamente o usuário**, ou  
- **redirecionar para a tela de login**

Isso evita confusão no processo de cadastro e melhora a experiência do usuário.

---

### 5. Padronização visual e de tipografia

Padronizar elementos da interface, como:

- estilo dos **labels**
- **espaçamento** entre campos
- comportamento de **botões e interações**

Essa padronização melhora a **consistência visual e a legibilidade da interface**.

---

### 6. Melhorar logs e tratamento de erros

O sistema poderia registrar **logs mais informativos** no console ou no backend quando ocorrerem falhas inesperadas.

Isso facilita o **diagnóstico, manutenção e investigação de problemas** durante desenvolvimento e testes.
