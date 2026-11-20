# Vistoria do percurso · atendimento das ocorrências (Maringá)

Painel operacional da vistoria do percurso da corrida de rua (vistoria de 19/11/2026, Despacho SEI 10096175).

- **Site estático** (GitHub Pages): um único `index.html`, com o Leaflet e o mapa de calor embutidos. Fundo do mapa: OpenStreetMap.
- **Dados dos pontos**: ficam dentro do `index.html` (bloco `<script id="data">`). São 48 pontos de atendimento, consolidados de 76 demandas do despacho, com frente, quadrante, tipo, link do Google Maps e observações.
- **Status** (Programada / Atendida): gravado no Supabase, tabela `percurso_status`. Qualquer pessoa com o link lê; para mudar o status é preciso o PIN da coordenação e um nome, que fica registrado em `percurso_historico`.

## Links com filtro pronto

O endereço guarda os filtros. Exemplos:

- `…/#frente=1` · `…/#frente=2` · `…/#frente=3`
- `…/#quadrante=NE` (NO, NE, SO, SE)
- `…/#tipo=comb` (pontos com buraco) · `…/#tipo=coml` (pontos com limpeza)
- `…/#frente=2&status=Programada` (o que falta na Frente 2)

## Manutenção (SQL no projeto Supabase)

Trocar o PIN:

```sql
update percurso_priv.config
   set pin_hash = extensions.crypt('NOVO_PIN', extensions.gen_salt('bf', 8));
```

Zerar todos os status:

```sql
update public.percurso_status set status = 'Programada', atualizado_por = null, atualizado_em = null;
```

Ver quem mudou o quê:

```sql
select * from public.percurso_historico order by em desc;
```
