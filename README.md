# Marcus Segundo — Site

Landing page pessoal e de consultoria de **Marcus Segundo** — Gerente Sênior de Tecnologia, Data & AI, Open Finance e Cloud.

🔗 **https://marvinmvns.github.io**

## Sobre
Página única, estática e autocontida, com:
- Posicionamento de liderança em Data & AI + experiência de mercado (setor bancário, Open Finance/Insurance, pagamentos, etc.)
- Case de resultado (FinOps -60% sem perder SLA), conhecimentos técnicos, trajetória, projetos (DoguIA, Marvin, FastrackGPS, Harlio), vídeos e depoimentos
- Seletor de idiomas **PT / EN / ES**
- Hero com cena 3D (Three.js), terminal e retrato com efeito de glitch

## Estrutura
```
index.html              página única (HTML + CSS + JS)
.nojekyll               serve os arquivos sem processamento do Jekyll
assets/
  img/                  galeria (1.jpg…8.png), retrato (marcus-foto.jpg) e doguia-cores
  logos/                logos de empresas e instituições de ensino
  icons/                ícones da stack técnica (svg)
  thumbs/               thumbnails dos vídeos do YouTube
  video/                hero-robot.mp4
  docs/                 cv-marcus-segundo.pdf (currículo)
```
> Three.js é carregado via CDN (cdnjs, r128), não fica versionado no repositório.
> Pendência: `assets/img/1.jpg` é referenciado pela galeria mas ainda não existe.

## Deploy (GitHub Pages)
Repositório `marvinmvns.github.io` — o GitHub Pages publica automaticamente a partir da **raiz do branch padrão**. Qualquer `commit` na raiz atualiza o site em ~1 minuto.

## Tecnologias
HTML, CSS e JavaScript puro · Three.js (hero 3D) · sem etapa de build.
