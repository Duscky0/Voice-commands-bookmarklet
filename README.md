# Voice Commands Bookmarklet

Controle páginas da web com comandos de voz em **português**.  
Permite rolar a página ou fazer pesquisas no Google sem usar o teclado.

## 🎙️ Comandos disponíveis
- **Suba** → rola a página para cima.  
- **Desça** → rola a página para baixo.  
- **Pare** → interrompe o movimento.  
- **Pesquise [termo]** → abre uma nova guia com a pesquisa no Google.

## 🧠 Como usar
1. Copie o código abaixo.  
2. Crie um novo favorito no navegador.  
3. No campo **URL**, cole o código completo (começando com `javascript:`).  
4. Clique no favorito e fale os comandos.

## 💻 Código
```js
javascript:(()=>{
  const synth = window.speechSynthesis;
  const rec = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
  rec.lang = 'pt-BR';
  rec.continuous = true;
  rec.start();

  let scrollInterval;

  rec.onresult = e => {
    const comando = e.results[e.resultIndex][0].transcript.trim().toLowerCase();
    console.log('Comando:', comando);

    if (comando.includes('suba')) {
      clearInterval(scrollInterval);
      scrollInterval = setInterval(()=>window.scrollBy(0,-50),100);
    } 
    else if (comando.includes('desça')) {
      clearInterval(scrollInterval);
      scrollInterval = setInterval(()=>window.scrollBy(0,50),100);
    } 
    else if (comando.includes('pare')) {
      clearInterval(scrollInterval);
    } 
    else if (comando.startsWith('pesquise')) {
      clearInterval(scrollInterval);
      const termo = comando.replace('pesquise','').trim();
      if(termo) window.open('https://www.google.com/search?q='+encodeURIComponent(termo),'_blank');
    }
  };

  synth.speak(new SpeechSynthesisUtterance('Reconhecimento ativado'));
})();
