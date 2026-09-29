---
tipo: aprendizado
data: 2026-09-29
contexto: "[[2026-09-29-pr-035-configuracoes-empresa]] (e antes [[2026-09-29-pr-024-editar-assinatura]])"
tags: [aprendizado, frontend, formularios]
---

# Valor inicial de formulário é uma cópia tirada ao montar

## O sintoma

O console mostra, ao salvar:

```
Base UI: A component is changing the default value state of an uncontrolled FieldControl after being
initialized. To suppress this warning opt to use a controlled FieldControl.
```

O mesmo aviso aparece para o `Select`. A tela funciona, mas o console deixa de estar limpo, que é o
critério de todo teste de navegador do projeto.

## A causa

O formulário não é controlado: cada campo recebe `defaultValue` com o dado gravado, por exemplo
`organization.name`. Salvar grava no repositório, e o `useSyncExternalStore` entrega o dado novo. O
`defaultValue` muda, e o Base UI avisa, porque valor inicial não deveria mudar depois de montado.

## A correção

Tirar uma cópia do dado ao montar e usá-la como valor inicial:

```tsx
const [organization] = useState(current); // a cópia não muda quando o repositório muda
```

Para trocar de registro (outra empresa, outra assinatura), refazer o formulário pela `key`:

```tsx
<OrganizationForm key={session.organization.id} organization={session.organization} />
```

## Como evitar

- Formulário de edição de dado do repositório: valor inicial sempre de uma cópia tirada ao montar, e
  `key` com o id do registro.
- Aconteceu no diálogo de edição de assinatura ([[2026-09-29-pr-024-editar-assinatura]]) e nas
  configurações da empresa ([[2026-09-29-pr-035-configuracoes-empresa]]).
