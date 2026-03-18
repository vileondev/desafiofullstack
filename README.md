# Desafio Fullstack

### Modelagem das Tabelas:
- A estrutura do banco foi pensada para ser simples, escalável e bem relacionada, garantindo integridade dos dados e facilidade de manutenção:
```sql
CREATE TABLE checklists_templates (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    titulo VARCHAR(255) NOT NULL,
    criado_por UUID REFERENCES auth.users(id) ON DELETE CASCADE,
    criado_em TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE TABLE checklist_etapas (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    id_checklist_template UUID REFERENCES checklists_templates(id) ON DELETE CASCADE,
    descricao TEXT NOT NULL,
    requer_foto BOOLEAN DEFAULT FALSE,
    ordem INTEGER NOT NULL
);

CREATE TABLE atribuicoes (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    id_checklist_template UUID REFERENCES checklists_templates(id) ON DELETE CASCADE,
    id_staff UUID REFERENCES auth.users(id) ON DELETE CASCADE,
    status VARCHAR(50) DEFAULT 'pendente' -- pendente, concluido
);

CREATE TABLE execucoes (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    id_atribuicao UUID REFERENCES atribuicoes(id) ON DELETE CASCADE,
    enviado_em TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    status_auditoria VARCHAR(50) DEFAULT 'pendente_validacao' -- pendente_validacao, aprovado, rejeitado
);

CREATE TABLE resultados_etapas (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    id_execucao UUID REFERENCES execucoes(id) ON DELETE CASCADE,
    id_etapa UUID REFERENCES checklist_etapas(id) ON DELETE CASCADE,
    foto_url TEXT NULL,
    justificativa TEXT NULL,
    CONSTRAINT valida_submissao CHECK (foto_url IS NOT NULL OR justificativa IS NOT NULL)
);
```
## Contorno aos limite de créditos da IA (V0)

Para lidar com as limitações de crédito, optei por uma abordagem mais estratégica e consciente no uso da IA. Evitei desperdício de tokens ao máximo, focando apenas em prompts realmente essenciais. Isso exigiu uma engenharia de prompts mais refinada. Cada solicitação precisava ser clara, objetiva e bem pensada antes de ser executada. Outro ponto importante foi a escolha do modelo. Pensando em custo-benefício, utilizei o v0 Pro, que entregava bons resultados sem consumir créditos de forma exagerada. Além disso, contei com o apoio de outros agentes, como o Gemini, para revisar e melhorar os prompts. Essa colaboração ajudou a extrair respostas mais completas com menos tentativas. Mas, sem dúvida, o fator mais decisivo foi a preparação: documentar toda a estrutura do aplicativo antes de começar. Isso reduziu drasticamente retrabalho e uso desnecessário da IA. [Acessar Documentação](v0.md).

## Resumo da Jornada

Essa experiência foi muito além de apenas “codar”. Ela reforçou na prática a importância do conceito de Build to Learn — aprender construindo, mas com intenção e estratégia. Percebi que, antes de sair escrevendo código (mesmo com ajuda de IA), é fundamental parar, pensar e estruturar bem o projeto. Essa etapa de planejamento, muitas vezes ignorada, faz toda a diferença no resultado final.

Mesmo em um cenário de Vibe Coding, onde tudo parece mais rápido e informal, a base continua sendo a mesma: clareza, organização e visão de longo prazo. Na minha visão, essa habilidade de estruturar antes de executar é algo essencial para qualquer desenvolvedor em formação, seja no ensino técnico, na faculdade ou de forma autodidata.
