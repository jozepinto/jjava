# Instruções para Testar o Sistema

## ✅ Correções Realizadas

Todas as conexões entre classes foram corrigidas:

1. **Inscricao** → Usa `Participante` (antes usava `Person`)
2. **Evento** → Usa `Promotor` (antes usava `Person`)
3. **Palestra** → Construtor correto com `Promotor`
4. **OutroEvento** → Construtor correto com `Promotor`
5. **Pedido** → Usa `Promotor` (antes usava `Person`)
6. **Services** → Todos atualizados para usar os repositórios corretos

## 🚀 Como Executar

### Passo 1: Compilar o Projeto

**Opção A - IntelliJ IDEA:**
1. Abra o projeto no IntelliJ
2. Clique em `Build` → `Build Project` (ou pressione `Ctrl+F9`)
3. Verifique se não há erros de compilação no painel inferior

**Opção B - VS Code:**
1. Abra o projeto no VS Code
2. A extensão Java já deve compilar automaticamente
3. Verifique o painel de problemas (Problems)

**Opção C - Terminal (se Java estiver configurado):**
```bash
./mvnw.cmd clean install -DskipTests
```

### Passo 2: Executar a Aplicação Spring Boot

1. Localize o arquivo: `src/main/java/upt/equipa05_comp2/equipa05_comp2/Equipa05Comp2Application.java`
2. Clique com botão direito → `Run 'Equipa05Comp2Application'`
3. Aguarde a mensagem: `Started Equipa05Comp2Application in X seconds`
4. A aplicação estará rodando em: `http://localhost:8080`

### Passo 3: Testar o Menu Cliente

1. **MANTENHA** a aplicação Spring Boot rodando
2. Em outra janela/terminal, execute: `src/main/java/upt/equipa05_comp2/equipa05_comp2/Client/MainClient.java`
3. Faça login com um usuário existente ou crie um novo

## 🧪 Testes Sugeridos

### Teste 1: Login como Admin
1. Execute o `MainClient`
2. Login como Admin (se tiver um usuário admin criado)
3. Teste: Registar um novo Promotor
4. Teste: Registar um novo Participante

### Teste 2: Criar Pedido (como Promotor)
1. Login como Promotor
2. Escolha opção "Criar Pedido"
3. Preencha TODOS os dados solicitados:
   - Título, Nome Promotores, Data, Duração
   - Local, Descrição
   - Tipo de Evento (escolha 1-4)
   - Departamento (escolha 1-4)
   - Orçamento, Capacidade
4. Verifique se o pedido foi criado com sucesso

### Teste 3: Avaliar Pedido (como Gestor)
1. Login como Gestor
2. Liste os pedidos
3. Avalie um pedido (aprovar ou rejeitar)

### Teste 4: Criar Evento (como Promotor)
1. Login como Promotor
2. Selecione um pedido APROVADO
3. Formalize o pedido (criar evento)
   - Preencha os dados adicionais: Hora, Preço, Email, Contacto, Oradores, etc.
4. Verifique se o evento foi criado

### Teste 5: Inscrição em Evento (como Participante)
1. Login como Participante
2. Consulte eventos disponíveis
3. Inscreva-se em um evento
4. Verifique suas inscrições

## 🔍 Verificações Importantes

### Verificar Banco de Dados
Se estiver usando H2 Console:
1. Acesse: `http://localhost:8080/h2-console`
2. JDBC URL: `jdbc:h2:mem:testdb` (verifique em application.properties)
3. Verifique as tabelas:
   - `person` (deve ter discriminador para Promotor/Participante)
   - `eventos` (deve ter discriminador para Palestra/OutroEvento)
   - `inscricoes` (deve ter foreign keys para participante e evento)
   - `pedidos` (deve ter foreign key para promotor)

### Estrutura de Relacionamentos Esperada

```
Person (superclass)
├── Promotor
│   ├── eventos (OneToMany)
│   └── pedidos (OneToMany)
└── Participante
    └── inscricoes (OneToMany)

Evento (superclass)
├── promotor (ManyToOne → Promotor)
├── inscricoes (OneToMany)
└── Subclasses:
    ├── Palestra
    └── OutroEvento

Inscricao
├── participante (ManyToOne → Participante)
└── evento (ManyToOne → Evento)

Pedido
└── promotor (ManyToOne → Promotor)
```

## ⚠️ Possíveis Erros e Soluções

### Erro: "Port 8080 already in use"
**Solução:** Outra aplicação está usando a porta 8080. Pare-a ou altere a porta em `application.properties`:
```properties
server.port=8081
```

### Erro: "Failed to configure a DataSource"
**Solução:** Verifique as configurações do banco de dados em `application.properties`

### Erro: "Could not create connection to database server"
**Solução:** Se estiver usando MySQL/PostgreSQL, verifique se o servidor está rodando

### Erro de Compilação
**Solução:** Certifique-se de que todas as dependências estão baixadas. Execute:
```bash
./mvnw.cmd clean install -U
```

## 📝 Notas Finais

- Todas as correções foram aplicadas nos **modelos**, **services** e mantêm compatibilidade com o **menu existente**
- O sistema agora segue corretamente a hierarquia de herança
- As relações entre classes estão semanticamente corretas:
  - Participante → apenas se inscreve em eventos
  - Promotor → cria pedidos e eventos
  - Inscrição → liga participante ao evento
  
**Status:** ✅ Sistema corrigido e pronto para uso!
