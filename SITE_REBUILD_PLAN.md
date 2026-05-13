# Plano de Rebuild do Site TYVE
**Criado:** 13/05/2026 | **Status:** Em execução

## Estado atual
- `index.html` — **LIMPO** ✅ Apenas topo novo (Block 2 CSS + nav + hero + showcase + footer mínimo). 1 style block. Sem colisão.
- `preview-antigo.html` — **Referência para João** ✅ Site antigo completo, 1 style block, sem colisão. URL: `tyve.com.br/preview-antigo`

## Regra absoluta para implementar seções novas do João

### 1. CSS sempre scoped — NUNCA classe genérica solta
```css
/* ❌ ERRADO — pode colidir com qualquer coisa */
.title { color: #0B0F14; }
.card { border-radius: 12px; }

/* ✅ CERTO — prefixo único por seção */
.s-features .title { color: #0B0F14; }
.s-features .card { border-radius: 12px; }
```

### 2. Variáveis CSS: NUNCA redefinir :root — usar scoped vars
```css
/* ❌ ERRADO — sobrescreve variáveis globais */
:root { --orange: #f84100; }

/* ✅ CERTO — variável só afeta aquela seção */
.s-nova-secao { --orange: #f84100; }
```

### 3. Prefixo obrigatório por seção
| Seção João | Prefixo classe |
|---|---|
| Como funciona | `.s-how` |
| Para quem é | `.s-forwho` |
| Features / Na prática | `.s-features` |
| Planos e Preços | `.s-pricing` |
| Comparativo VS | `.s-vs` |
| Depoimentos | `.s-testimonial` |
| ROI Calculator | `.s-roi` |
| FAQ | `.s-faq` |
| Final CTA | `.s-cta` |
| Footer | `.s-footer` |

### 4. Checklist antes de cada commit de seção nova
- [ ] CSS da seção tem prefixo `.s-[nome]`?
- [ ] `:root` não foi redefinido?
- [ ] Testado visualmente no browser sem outras seções afetadas?
- [ ] Classe nova não existe em nenhum outro `<style>` do arquivo?

## Checklist diário (verificar toda sessão que tocar no site)

```bash
# 1. Contar style blocks — deve ser SEMPRE 1
grep -c "<style" site/index.html

# 2. Conferir que não tem CSS de seções antigas
grep -c "class=\"problem\|class=\"how\|class=\"pricing\|class=\"faq" site/index.html

# 3. Verificar que não tem :root redefinido fora do primeiro bloco
grep -n ":root" site/index.html
```

## Workflow para cada seção nova do João

1. **João entrega:** screenshot / Figma / HTML de uma seção
2. **Eu implemento:**
   - Adiciono o HTML da seção após `.showcase` (ou onde couber)
   - CSS vai no único `<style>` existente, com prefixo `.s-[nome]`
3. **Verificação:** rodar checklist acima antes de commitar
4. **Deploy:** `cd site && git add . && git commit -m "add: seção [nome]" && git push`
5. **Confirmar live:** checar `tyve.com.br` no browser

## Estado das seções (atualizar conforme João entregar)
| Seção | Status | Observações |
|---|---|---|
| Nav | ✅ Implementado | Logo João, laranja |
| Hero | ✅ Implementado | Headline + Luma + chat mockup |
| Showcase | ✅ Implementado | Foto Luma + chat animado |
| Como funciona | ⏳ Aguardando João | Referência: preview-antigo.html |
| Para quem é | ⏳ Aguardando João | |
| Features / Na prática | ⏳ Aguardando João | |
| Planos e Preços | ⏳ Aguardando João | |
| Comparativo VS | ⏳ Aguardando João | |
| Depoimentos | ⏳ Aguardando João | |
| ROI Calculator | ⏳ Aguardando João | |
| FAQ | ⏳ Aguardando João | |
| Final CTA | ⏳ Aguardando João | |
| Footer | ⏳ Aguardando João | Footer mínimo placeholder hoje |

## Limpeza final (quando todas as seções estiverem prontas)
1. Remover `preview-antigo.html` (não precisa mais estar no deploy)
2. Remover variáveis CSS não usadas do `:root`
3. Checar mobile em Chrome DevTools
4. Checar i18n PT/EN em todas as seções novas
5. Commit final: `git commit -m "feat: site completo nova marca"`
