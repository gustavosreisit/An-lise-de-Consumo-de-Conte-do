📊 Análise de Consumo de Conteúdo
📌 Sobre o Projeto

Este projeto tem como objetivo analisar o comportamento de usuários em uma plataforma de streaming, explorando o consumo de conteúdo com base em:

Faixa etária
Tipo de conteúdo
Tempo assistido
Plano do usuário

A análise foi realizada utilizando SQL para tratamento e transformação dos dados, com integração em Power BI para visualização.

🧠 Regras de Negócio Aplicadas
Classificação de usuários por faixa etária:
Menor de idade (< 18)
Jovem (18–29)
Adulto (30–59)
Idoso (60+)
Formatação do tempo de duração dos conteúdos:
Conteúdos com menos de 60 minutos → exibidos em minutos
Conteúdos com mais de 60 minutos → exibidos em horas e minutos
Padronização do nome dos planos (primeira letra maiúscula)
💻 Query SQL
SELECT 
    d.id_usuario,
    COALESCE(d.nome, 'Sem Nome') AS nome,

    CASE
        WHEN d.idade < 18 THEN 'Menor de idade'
        WHEN d.idade >= 18 AND d.idade < 30 THEN 'Jovem'
        WHEN d.idade >= 30 AND d.idade < 60 THEN 'Adulto'
        ELSE 'Idoso'
    END AS faixa_etaria,

    f.tempo_assistido_min,

    c.titulo,
    c.genero,

    CASE 
        WHEN c.duracao_min < 60 THEN CONCAT(c.duracao_min, ' min')
        ELSE CONCAT(
            c.duracao_min DIV 60, 'h ',
            c.duracao_min % 60, 'min'
        )
    END AS tempo_conteudo,

    CONCAT(
        UPPER(LEFT(f.plano,1)),
        LOWER(SUBSTRING(f.plano,2))
    ) AS plano

FROM fato_consumo_conteudo f
LEFT JOIN dim_usuario d 
    ON d.id_usuario = f.id_usuario 
LEFT JOIN dim_conteudo c 
    ON c.id_conteudo = f.id_conteudo

ORDER BY f.tempo_assistido_min DESC;
--
📊 Dashboard

Abaixo está o relatório desenvolvido no Power BI com os principais insights da análise:

<img width="1248" height="717" alt="image" src="https://github.com/user-attachments/assets/128b68ff-315d-4818-a556-b32263962b54" />


📊 Possíveis Insights
Identificação de quais faixas etárias consomem mais conteúdo
Conteúdos mais assistidos por gênero
Relação entre plano e tempo de consumo
Padrões de comportamento dos usuários
--
🛠️ Tecnologias Utilizadas
SQL
Power BI

📬 Autor: Gustavo Silva Reis
🔗 LinkedIn: https://www.linkedin.com/in/gsreisit
