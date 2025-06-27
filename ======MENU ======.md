```dataviewjs
// Definiujemy foldery dla kolumn (jeśli ścieżka jest pusta, kolumna nie zostanie stworzona)
const folders = {
  RedTeam: "NOTES/0. Redteam",
  Services: "NOTES/0. Redteam/2. Services",
  WebApp: "NOTES/0. Redteam/WebAPP",
};


// Funkcja generująca anchor linki z plików w danym folderze
function getFilesList(folderPath) {
  const folderObj = app.vault.getAbstractFileByPath(folderPath);
  if (!folderObj || !folderObj.children) {
    return "";
  }
  // Filtrujemy dzieci, aby pozostawić tylko pliki (pomijamy foldery)
  let files = folderObj.children.filter(child => !child.children);
  
  // Mapujemy pliki: usuwamy rozszerzenia .canvas oraz .md i wyciągamy numeryczny prefiks (do sortowania)
  let items = files.map(child => {
    let displayName = child.name.replace(/\.canvas/g, "").replace(/\.md/g, "");
    let numMatch = displayName.match(/^(\d+)/);
    let sortKey = numMatch ? parseInt(numMatch[1]) : Infinity;
    return { child, displayName, sortKey };
  });
  
  // Sortujemy pliki według numerycznego prefiksu
    items.sort((a, b) => {
    if (a.sortKey === b.sortKey) {
      return a.displayName.toLowerCase().localeCompare(b.displayName.toLowerCase());
    }
    return a.sortKey - b.sortKey;
  });
  
  let out = "";
  let vaultName = app.vault.getName();
  items.forEach(item => {
    // Tworzymy link używając schematu obsidian://open
    let link = `obsidian://open?vault=${encodeURIComponent(vaultName)}&file=${encodeURIComponent(item.child.path)}`;
    out += `<a href="${link}">${item.displayName}</a><br/>`;
  });
  return out;
}


// Funkcja tworząca kolumnę z nagłówkiem i zawartością
function createColumn(header, content) {
  let col = dv.el("div","");
  //col.style.flex = "1"; //odnośniki będą łamać się do następnej linii jeśli będą zbyt długie
  //col.style.border = "10px solid var(--background-modifier-border)"; //dodaj ramkę o grubości 10px wokół poszczególnych elementów mających być obok siebie
  col.style.padding = "1px";
  //col.style.borderRadius = "4px"; //zaokrąglenie rogów border
  
  let h = dv.el("h1", header);
  col.appendChild(h);
  
  let divContent = dv.el("div");
  divContent.innerHTML = content;
  col.appendChild(divContent);
  
  return col;
}



// Tworzymy kontener z flexboxa
let container = dv.el("div","");
container.style.display = "flex";
//container.style.gap = "10px";

// Iterujemy po obiekcie folders – dla każdej niepustej ścieżki tworzymy kolumnę
for (const key in folders) {
  let folderPath = folders[key];
  if (folderPath && folderPath.trim() !== "") {
    let content = getFilesList(folderPath);
    container.appendChild(createColumn(key, content));
  }
}

// Renderujemy kontener w notatce
dv.container.appendChild(container);
```


---

```dataviewjs
const folders = {
  Linux: "NOTES/1. Linux",     // przykładowo – jeżeli folder istnieje
  Windows: "NOTES/2. Windows", // przykładowo – jeżeli folder istnieje
  AD: "NOTES/2. Windows/AD"    // pusta wartość – kolumna nie zostanie stworzona
};




// Funkcja generująca anchor linki z plików w danym folderze
function getFilesList(folderPath) {
  const folderObj = app.vault.getAbstractFileByPath(folderPath);
  if (!folderObj || !folderObj.children) {
    return "";
  }
  // Filtrujemy dzieci, aby pozostawić tylko pliki (pomijamy foldery)
  let files = folderObj.children.filter(child => !child.children);
  
  // Mapujemy pliki: usuwamy rozszerzenia .canvas oraz .md i wyciągamy numeryczny prefiks (do sortowania)
  let items = files.map(child => {
    let displayName = child.name.replace(/\.canvas/g, "").replace(/\.md/g, "");
    let numMatch = displayName.match(/^(\d+)/);
    let sortKey = numMatch ? parseInt(numMatch[1]) : Infinity;
    return { child, displayName, sortKey };
  });
  
  // Sortujemy pliki według numerycznego prefiksu
    items.sort((a, b) => {
    if (a.sortKey === b.sortKey) {
      return a.displayName.toLowerCase().localeCompare(b.displayName.toLowerCase());
    }
    return a.sortKey - b.sortKey;
  });
  
  let out = "";
  let vaultName = app.vault.getName();
  items.forEach(item => {
    // Tworzymy link używając schematu obsidian://open
    let link = `obsidian://open?vault=${encodeURIComponent(vaultName)}&file=${encodeURIComponent(item.child.path)}`;
    out += `<a href="${link}">${item.displayName}</a><br/>`;
  });
  return out;
}


// Funkcja tworząca kolumnę z nagłówkiem i zawartością
function createColumn(header, content) {
  let col = dv.el("div","");
  //col.style.flex = "1"; //odnośniki będą łamać się do następnej linii jeśli będą zbyt długie
  //col.style.border = "10px solid var(--background-modifier-border)"; //dodaj ramkę o grubości 10px wokół poszczególnych elementów mających być obok siebie
  col.style.padding = "1px";
  //col.style.borderRadius = "4px"; //zaokrąglenie rogów border
  
  let h = dv.el("h1", header);
  col.appendChild(h);
  
  let divContent = dv.el("div");
  divContent.innerHTML = content;
  col.appendChild(divContent);
  
  return col;
}



// Tworzymy kontener z flexboxa
let container = dv.el("div","");
container.style.display = "flex";
//container.style.gap = "10px";

// Iterujemy po obiekcie folders – dla każdej niepustej ścieżki tworzymy kolumnę
for (const key in folders) {
  let folderPath = folders[key];
  if (folderPath && folderPath.trim() !== "") {
    let content = getFilesList(folderPath);
    container.appendChild(createColumn(key, content));
  }
}

// Renderujemy kontener w notatce
dv.container.appendChild(container);
```


```dataviewjs
// Definiujemy foldery dla kolumn (jeśli ścieżka jest pusta, kolumna nie zostanie stworzona)
const folders = {
  Tools: "NOTES/99. TOOLS/*",
};


// Funkcja generująca anchor linki z plików w danym folderze
function getFilesList(folderPath) {
  const folderObj = app.vault.getAbstractFileByPath(folderPath);
  if (!folderObj || !folderObj.children) {
    return "";
  }
  // Filtrujemy dzieci, aby pozostawić tylko pliki (pomijamy foldery)
  let files = folderObj.children.filter(child => !child.children);
  
  // Mapujemy pliki: usuwamy rozszerzenia .canvas oraz .md i wyciągamy numeryczny prefiks (do sortowania)
  let items = files.map(child => {
    let displayName = child.name.replace(/\.canvas/g, "").replace(/\.md/g, "");
    let numMatch = displayName.match(/^(\d+)/);
    let sortKey = numMatch ? parseInt(numMatch[1]) : Infinity;
    return { child, displayName, sortKey };
  });
  
  // Sortujemy pliki według numerycznego prefiksu
    items.sort((a, b) => {
    if (a.sortKey === b.sortKey) {
      return a.displayName.toLowerCase().localeCompare(b.displayName.toLowerCase());
    }
    return a.sortKey - b.sortKey;
  });
  
  let out = "";
  let vaultName = app.vault.getName();
  items.forEach(item => {
    // Tworzymy link używając schematu obsidian://open
    let link = `obsidian://open?vault=${encodeURIComponent(vaultName)}&file=${encodeURIComponent(item.child.path)}`;
    out += `<a href="${link}">${item.displayName}</a><br/>`;
  });
  return out;
}


// Funkcja tworząca kolumnę z nagłówkiem i zawartością
function createColumn(header, content) {
  let col = dv.el("div","");
  //col.style.flex = "1"; //odnośniki będą łamać się do następnej linii jeśli będą zbyt długie
  //col.style.border = "10px solid var(--background-modifier-border)"; //dodaj ramkę o grubości 10px wokół poszczególnych elementów mających być obok siebie
  col.style.padding = "1px";
  //col.style.borderRadius = "4px"; //zaokrąglenie rogów border
  
  let h = dv.el("h1", header);
  col.appendChild(h);
  
  let divContent = dv.el("div");
  divContent.innerHTML = content;
  col.appendChild(divContent);
  
  return col;
}



// Tworzymy kontener z flexboxa
let container = dv.el("div","");
container.style.display = "flex";
//container.style.gap = "10px";

// Iterujemy po obiekcie folders – dla każdej niepustej ścieżki tworzymy kolumnę
for (const key in folders) {
  let folderPath = folders[key];
  if (folderPath && folderPath.trim() !== "") {
    let content = getFilesList(folderPath);
    container.appendChild(createColumn(key, content));
  }
}

// Renderujemy kontener w notatce
dv.container.appendChild(container);
```