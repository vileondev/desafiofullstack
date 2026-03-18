# Desafio Fullstack

### Modelagem das Tabelas

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
´´´

