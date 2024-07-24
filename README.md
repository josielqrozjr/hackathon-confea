# hackathon-confea

# Documentação Técnica: Gateway de Integração para Emissão de ART

## Introdução

Atualmente, os profissionais de engenharia, agronomia e geociências precisam realizar um recadastro para atuar em outro estado, o que gera burocracia, retrabalho e duplicação de dados nos Conselhos Regionais envolvidos. Por exemplo, um engenheiro registrado no CREA-PR que deseja trabalhar em São Paulo precisa reenviar seus documentos para o CREA-SP, repetindo um processo já realizado na emissão do seu Registro Nacional do Profissional (RNP).

Para resolver este problema, nossa solução propõe um Gateway de Integração, que atuará como um orquestrador no tratamento das informações necessárias para emissão de Anotações de Responsabilidade Técnica (ART) e atualizações cadastrais, facilitando a comunicação entre os Conselhos Regionais.

## Funcionamento do Gateway

### Processo de Emissão de ART

1. **Solicitação de Emissão de ART**
   - O profissional acessa a plataforma centralizada com suas credenciais RNP.
   - Preenche o formulário padrão para emissão da ART, especificando a obra/serviço e sua localização.

2. **Envio de Dados para o Gateway**
   - A plataforma envia os dados da ART para o Gateway de Integração.

3. **Intermediação pelo Gateway**
   - O Gateway identifica a jurisdição do CREA responsável (baseado na localização da obra/serviço).
   - O Gateway encaminha a solicitação ao CREA de origem do profissional para validação dos dados.

4. **Validação e Aprovação**
   - O CREA de origem valida os dados e responde ao Gateway.
   - O Gateway repassa a informação para o CREA responsável pela emissão da ART.

5. **Emissão da ART**
   - O CREA responsável processa a emissão da ART e envia o documento para o Gateway.
   - O Gateway armazena o documento e o disponibiliza na plataforma centralizada.

6. **Notificação ao Profissional**
   - O profissional é notificado e pode acessar a ART emitida através da plataforma.

### Atualizações Cadastrais

1. **Solicitação de Atualização**
   - O profissional solicita uma atualização cadastral na plataforma centralizada.

2. **Processamento pelo Gateway**
   - O Gateway direciona a solicitação ao CREA de origem do profissional para validação e atualização dos dados.

3. **Sincronização dos Dados**
   - O CREA de origem valida e atualiza os dados.
   - O Gateway sincroniza a atualização com os outros CREAs envolvidos, evitando duplicação e inconsistências.

## Tratamento das Informações

### Busca
- O Gateway realiza consultas aos bancos de dados dos CREAs para obter informações necessárias.
- Utiliza APIs seguras para comunicação com os sistemas regionais.

### Entrega
- O Gateway entrega os dados solicitados de forma eficiente, garantindo que as informações sejam precisas e atualizadas.

### Comparação
- O Gateway compara os dados recebidos com as informações existentes para evitar duplicidades e inconsistências.

### Armazenamento em Cache
- Dados frequentemente acessados são armazenados em cache para melhorar a performance e reduzir a latência das operações.
- Utiliza técnicas de invalidação de cache para garantir que as informações estejam sempre atualizadas.

## Exemplo de Trajetória do Profissional

“Através do Gateway, reformulando a trajetória do profissional: Josiel, um engenheiro registrado no CREA-PA, recebeu uma oportunidade de trabalho no Rio Grande do Sul. Josiel não precisará reenviar seus dados, pois, ao emitir a ART, o CREA-RS enviará uma solicitação de dados para o Gateway, que direcionará a requisição para o conselho de origem do profissional, o CREA-PA.”

## Segurança

### Criptografia
- Toda a comunicação entre o Gateway, os CREAs e a plataforma centralizada é criptografada utilizando TLS/SSL para garantir a confidencialidade e integridade dos dados.

### Servidores
- **Servidor de RDP**: Utilizado para manutenção segura dos servidores.
- **Servidor de Código Aberto para RDP e Aplicação**: Hospeda a aplicação e permite o acesso remoto seguro.
- **Servidor de Banco de Dados**: Armazena os dados em cache, é fechado para a internet e acessível apenas pelo RDP e pela aplicação.
- **Servidor de Aplicação Web**: Aberto para a internet, hospeda a plataforma centralizada e as APIs que comunicam com os CREAs.

## Arquitetura Técnica

```plaintext
                         +---------------------------+
                         |                           |
                         |    Plataforma Central     |
                         |      (Web e Mobile)       |
                         |                           |
                         +-------------+-------------+
                                       |
                                       v
                         +---------------------------+
                         |                           |
                         |        Gateway de         |
                         |        Integração         |
                         |                           |
                         +-------------+-------------+
                                       |
           +---------------------------+---------------------------+
           |                           |                           |
   +---------------+           +---------------+           +---------------+
   |    CREA-PA    |           |    CREA-RS    |           |    CREA-SP    |
   +---------------+           +---------------+           +---------------+
           |                           |                           |
           |      APIs RESTful         |      APIs RESTful         |
           |                           |                           |
   +---------------+           +---------------+           +---------------+
   |   Middleware  |           |   Middleware  |           |   Middleware  |
   +---------------+           +---------------+           +---------------+
           |                           |                           |
   +---------------+           +---------------+           +---------------+
   | Banco de Dados|           | Banco de Dados|           | Banco de Dados|
   +---------------+           +---------------+           +---------------+
```

## Conclusão

A implementação do Gateway de Integração resolverá os problemas de fragmentação, redundância de dados e burocracia, proporcionando uma experiência eficiente e integrada para os profissionais registrados nos CREAs de todo o Brasil. Esta solução centraliza a emissão de ARTs e atualizações cadastrais, garantindo segurança, consistência e agilidade nos processos administrativos.
