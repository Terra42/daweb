Čím jsou naše programy větší a užitečnější v reálném životě, tím větší je objem a složitost informací, se kterými potřebují pracovat. Informacím, se kterými program pracuje říkáme data. Jednoduchá data v naších programech reprezentujeme pomocí hodnot jako čísla, řetězce, pravdivostní hodnoty apod. Brzy ale narazíme na komplexnější data, která mají nějakou složitější vnitřní strukturu. K reprezentaci takových dat používáme různé <term cs="datové struktury" en="data structures">. V tomto kurzu jsme zatím viděli pouze jednu takovou strukturu a tou je pole. Dnes si ukážeme další strukturu zvanou <term cs="objekt" en="object">, která na začátku vypadá zcela nevinně, nakonec však navždy změní naše životy a programování už nikdy nebude to, co bývalo dříve.

## Objekty jako data

Pokud chceme reprezentovat složitější data, i obyčejná pole nám nabízejí dostatak prostoru. Vzpomeňte si například na naši tabulku útrat.

```js
const expenses = [
  ['Petr', 'Prací prášek', 240],
  ['Ondra', 'Savo', 80],
  ['Pavla', 'Toaleťák', 65],
  ['Zuzka', 'Mýdlo', 50],
  ['Pavla', 'Závěs do koupelny', 350],
  ['Libor', 'Pivka na kolaudačku', 124],
  ['Petr', 'Pytle na odpadky', 75],
  ['Míša', 'Utěrky na nádobí', 130],
  ['Ondra', 'Toaleťák', 120],
  ['Míša', 'Pečící papír', 30],
  ['Zuzka', 'Savo', 80],
  ['Petr', 'Tapeta na záchod', 315],
  ['Ondra', 'Toaleťák', 64],
];
```

Představme si však, že strukturovanost těchto dat ještě naroste. Každý člověk v tabulce bude mít nejen jméno, ale i příjmení, u zakoupené věci budeme zaznamenávat také množství a jednotky. Pole můžeme libovolně vnořovat, není tedy problém naší strukturu předělat takto.

```js
const expenses = [
  [['Petr', 'Bílek'], ['Prací prášek', 1.5, 'kg'], 240],
  [['Ondřej', 'Zvěřina'], ['Savo', 1, 'ks'], 80],
  [['Pavla', 'Durdíková'], ['Toaleťák', 1, 'balení'], 65],
  [['Zuzana', 'Kaczynská'], ['Mýdlo', 200, 'ml'], 50],
  [['Pavla', 'Durdíková'], ['Závěs do koupelny', 1, 'ks'], 350],
  [['Libor', 'Krejčí'], ['Pivka na kolaudačku', 40, 'ks'], 124],
  [['Petr', 'Bílek'], ['Pytle na odpadky', 3, 'balení'], 75],
  [['Michaela', 'Reischlová'], ['Utěrky na nádobí', 1, 'balení'], 130],
  [['Ondřej', 'Zvěřina'], ['Toaleťák', 2, 'balení'], 120],
  [['Michaela', 'Reischlová'], ['Pečící papír', 1, 'balení'], 30],
  [['Zuzana', 'Kaczynská'], ['Savo', 1, 'ks'], 80],
  [['Petr', 'Bílek'], ['Tapeta na záchod', 1, 'ks'], 315],
  [['Ondřej', 'Zvěřina'], ['Toaleťák', 1, 'balení'], 64],
];
```

Taková struktura už ale pomalu začíná být malinko nepřehledná. Pokud se chceme dostat k názvu zakoupené věci, musíme napsat například.

```jscon
> expenses[3][1][0]
'Mýdlo'
```

Z tohoto výrazu už je dost těžké vyčíst, co vlastně z tabulky vytahujeme, musíme počítat indexy na prstech ruky a je velmi snadné se splést. Musíme si totiž pamatovat, že jméno produktu je na indexu 0, že produkt samotný je na indexu 1 a tak dále.

Abychom měli život o kus jednodušší, použijeme k reprezentaci řádku v tabulce místo pole objekt. Objekty ukládají data ve formátu klíč - hodnota. Jeden řádek naší tabulky (jednoduchá verze) pak bude vypadat takto.

```js
const row = {
  name: 'Petr',
  product: 'Prací prášek',
  price: 240,
};
```

Výhoda objektů spočívá v tom, že ke každé hodnotě se nyní dostaneme pomocí klíče a nemusíme řešit indexy.

```jscon
> row.name
'Petr'
> row.product
'Prací prášek'
> row.price
240
```

Objekt je nový typ hodnoty. Můžeme s ním tedy jako vždy provádět cokoliv, co děláme s ostatními hodnotami - ukládat do proměnné, používat ve výrazech, předávat jako vstupy funkcím apod. Díky tomu mohou objekty být součástí polí. Celá naše tabulka pak bude vypadat takto.

```js
const expenses = [
  {name: 'Petr', product: 'Prací prášek', price: 240},
  {name: 'Ondra', product: 'Savo', price: 80},
  {name: 'Pavla', product: 'Toaleťák', price: 65},
  {name: 'Zuzka', product: 'Mýdlo', price: 50},
  {name: 'Pavla', product: 'Závěs do koupelny', price: 350},
  {name: 'Libor', product: 'Pivka na kolaudačku', price: 124},
  {name: 'Petr', product: 'Pytle na odpadky', price: 75},
  {name: 'Míša', product: 'Utěrky na nádobí', price: 130},
  {name: 'Ondra', product: 'Toaleťák', price: 120},
  {name: 'Míša', product: 'Pečící papír', price: 30},
  {name: 'Zuzka', product: 'Savo', price: 80},
  {name: 'Petr', product: 'Tapeta na záchod', price: 315},
  {name: 'Ondra', product: 'Toaleťák', price: 6}]
];
```

Navíc, každý klíč v objektu může odkazovat na libovolnou hodnotu. V řádku tabulky zatím máme jen řetězce a číslo. Není však problém použít pole nebo rovnou další objekt. Můžeme tak vytvářen zanořené struktury jako například tuto.

```js
const row = {
  name: {
    first: 'Petr',
    last: 'Bílek',
  },
  product: {
    name: 'Prací prášek',
    amount: 1.5,
    unit: 'kg',
  },
  price: 240,
};
```

K názvu produktu se pak snadno dostaneme takto.

```jscon
> row.product.name
'Prací prášek'
```

To je jistě mnohem srozumitelnější, než používat indexu u polí. Díky objektům a polím můžeme vytvářet mnohonásobně složitější a komplikovanější struktury, které mohou reprezentovat téměř jakákoliv strukturovaná data.

**Tajemství**: Pokud může být pod klíčem v objektu livobolná hodnota, zvídavého člověka možná napade co by se tak stalo, kdybychom jako hodnotu v objektu použitli funkci. Funkce jsou přece také právoplatní hodnoty. Toto je ale právě ta věc, která navždy změní naše programátorské životy a na takovou změnu je třeba se duševně připravit. Odložíme ji proto zatím do některé z dalších lekci.

### Tečková notace

Pokud objekty zapisujeme způsobem jako výše, názvy klíčů se musí řídit stejnými pravidly jako názvy proměnných. Nemůžeme tak mít uvnitř názvu klíče mezeru nebo třeba pomlčku. Většinou se nám podaří takovým znakům vyhnout. Ne vždy je to ale možné. Pokud potřebujeme klíč s názvem, který porušuje pravidla JavaScriptových proměnných, stačí jej uzavřít do uvozovek.

```js
const person = {
  'first-name': 'Petr',
  'last-name': 'Bílek',
};
```

V takovémto případě ale nemůžeme k hodnotám v objektu přistupovat jako obvykle.

```jscon
> person.first-name;
```

JavaScript by si myslel, že od hodnoty `person.first` odečítáme hodnotu `name`. Musíme tedy použít alternativní způsob přistupu ke klíčům.

```jscon
> person['first-name'];
'Petr'
```

Tento způsob zdaleka není tak elegantní jako tečková notace. Proto se snažíme v názvech klíčů vyhýbat znakům, které nepatří do názvů proměnných a můžeme tak používat tečkovou notaci.

### Složitější struktury

Díky objektům a polím můžeme nyní v našich programech reprezentovat mnohem složitější data než dříve. Takto bychom mohli reprezentovat například kurz Czechitas jménem <i>Úvod do programování</i>.

```js
const course = {
  nazev: 'Úvod do programování',
  lektor: 'Martin Podloucký',
  konani: [
    {
      misto: 'T-Mobile',
      koucove: ['Dan Vrátil', 'Filip Kopecký', 'Martina Nemčoková'],
      ucastnic: 30,
    },
    {
      misto: 'MSD IT',
      koucove: ['Dan Vrátil', 'Zuzana Tučková', 'Martina Nemčoková'],
      ucastnic: 25,
    },
    {
      misto: 'Škoda DigiLab',
      koucove: ['Dan Vrátil', 'Filip Kopecký', 'Kateřina Kalášková'],
      ucastnic: 41,
    },
  ],
};
```

Všimněte si, jak objekt představující jeden kurz obsahuje pod klíčem `konani` pole dalších objektů. Každý z těchto objektů reprezentuje jedno konání kurzu a dále obsahuje například pole koučů, místo atp. Kdybychom tedy například chtěli seznam všech koučů na druhém konání kurzu, napsali bychom

```jscon
> course.konani[1].koucove
[ 'Dan Vrátil', 'Zuzana Tučková', 'Martina Nemčoková' ]
```

@exercises ## Cvičení - objekty jako data [

- kurz
- knihovna
  ]@

## Vlastní DOM elementy

Vraťme se na chvíli k našemu příkladu s nákupním seznamem z předchozí lekce.

```js
const shoppingList = [
  'mrkev',
  'paprika',
  'cibule',
  'čínské zelí',
  'arašídy',
  'sojová omáčka',
];

const updateShoppingList = () => {
  const listElm = document.querySelector('#shopping-list');
  listElm.innerHTML = '';
  for (let i = 0; i < shoppingList.length; i += 1) {
    listElm.innerHTML += `<li>${shoppingList[i]}</li>`;
  }
};
```

Dejme tomu, že bychom chtěli uživateli umožnit nějakou položku ze seznamu odstranit, aby tak mohl sledovat, co už koupil a co ještě zbývá. Pro jednoduchost řekněme, že položku odstraníme prostě tak, že na ni klikneme. To znamená, že musíme na každou položku seznamu pověsit posluchač událost `click`. Otázka zní, jak to udělat? Zkuste se nad ní na chvíli zamyslet, než budete pokračovat dál.

Možností existuje jístě vícero. Jedna z nich je například vytvořit celý seznam tak, jako výše, a pak vybrat všechny `li` elementy a pomocí cyklu na každý z nich pověsit posluchač.

```js
const updateShoppingList = () => {
  const listElm = document.querySelector('#shopping-list');
  listElm.innerHTML = '';
  for (let i = 0; i < shoppingList.length; i += 1) {
    listElm.innerHTML += `<li>${shoppingList[i]}</li>`;
  }

  const listItems = document.querySelectorAll('#shopping-list li');
  for (let i = 0; i < listItems.length; i += 1) {
    listItems[i].addEventListener('click', () => {
      console.log('item click');
    }
  }
};
```

Pro větši programy však takovýto přístup začne být lehce neohrabaný a těžko čitelný. Problém v podstatě spočívá v tom, že necháváme prohlížeč, aby vytvářel DOM element za nás tím, že nastavujeme `innerHTML`. Velmi se nám však uvolní ruce, pokud budeme schopni si DOM elementy tvořit sami.

### Jak vytvořit vlastní DOM element

Do této chvíle naše programy vždy fungovaly tak, že všechny DOM elementy, se kterými jsme pracovali, byly vybrány ze stránky pomocí `document.querySelector`. Nyní si ukážeme, jak vytvořit DOM element tak říkajíc na zelené louce **mimo** naši stránku. Takto vytvořený element pak můžeme různě opracovávat a zpojit jej do stránky až ve chvíli, kdy je hotový.

Nový DOM element vytvoříme pomocí funkce `document.createElement`. Takto například vytvoříme prázdný `li` element.

```js
const liElm = document.createElement('li');
```

V tuto chvíli máme vytvořený zcela plnoprávný DOM element, se kterým můžeme dělat všechno, co jsme s DOM elementy byli zvyklí dělali doposud: nastavovat `textContent`, měnit styly, měnit CSS třídy apod. Vytvořme tedy element pro naši položku nákupního seznamu, který bud obsahovat i tlačíko na smazání položky.

```js
const shopItemElm = document.createElement('li');
shopItemElm.className = 'shop-item';
shopItemElm.innerHTML = '<span>Jablka</span><button>koupeno</button>';
```

Pozor na to, že tento element zatím existuje zcela mimo naši stránku. Máme jej uložen pouze v proměnné `shopItemElm`. Pokud jej chcene do naší stránky zapojit, musíme nejdříve vybrat rodičovský element, dovnitř kterého náš nový element zapojíme. Na rodiči pak zavoláme metodu `appendChild`, která náš nový element zapojí jako poslední dítě tohoto rodiče.

V našem případě chceme naši nákupní položku přidat dovnitř seznamu s id `shopping-list`.

```js
const shopItemElm = document.createElement('li');
shopItemElm.className = 'shop-item';
shopItemElm.innerHTML = '<span>Jablka</span><button>koupeno</button>';

const listElm = document.querySelector('#shopping-list');
listElm.appendChild(shopItemElm);
```

Výhoda tohoto přístupu spočívá v tom, že můžeme na tlačítko v nášem elementu snadno připojit posluchače. Zde můžeme využít toho, že metodu `querySelector` nemusíme volat jen na celém dokumentu. Pokud ji zavoláme na DOM elementu, vybíráme pouze zevnitř něj a nikoliv z celého dokumentu. Snadno tak vyberme naše tlačíkto a pověsíme na něj posluchače.

```js
const shopItemElm = document.createElement('li');
shopItemElm.className = 'shop-item';
shopItemElm.innerHTML = '<span>Jablka</span><button>koupeno</button>';
const btnElm = shopItemElm.querySelector('button');
btnElm.addEventListener('click', () => {
  console.log('klik');
});

const listElm = document.querySelector('#shopping-list');
listElm.appendChild(shopItemElm);
```

### Funkce render

Vidíte sami, že příprava elementu pro položku nákupu zabere už notný kus kódu. Zde by bylo vhodné si jej zabalit do funkce. Funkce, které vytvářejí nějaký DOM element, je zvykem pojmenovávat slovíčkem `render`.

```js
const renderShopItem = (item) => {
  const shopItemElm = document.createElement('li');
  shopItemElm.className = 'shop-item';
  shopItemElm.innerHTML = `<span>${item}</span><button>koupeno</button>`;

  const btnElm = shopItemElm.querySelector('button');
  btnElm.addEventListener('click', () => {
    console.log('klik');
  });

  return shopItemElm;
};
```

Pokaždé, když zavoláme funkci `renderShopItem` s nějakým názvem položky, obdržíme nový DOM element představující naši nákupní položku. Ten pak můžeme zapojit do našeho nákupního seznamu. Celý program pak bude vypadat takto.

```js
const renderShopItem = (item) => {
  const shopItemElm = document.createElement('li');
  shopItemElm.className = 'shop-item';
  shopItemElm.innerHTML = `<span>${item}</span><button>koupeno</button>`;

  const btnElm = shopItemElm.querySelector('button');
  btnElm.addEventListener('click', () => {
    console.log('klik');
  });

  return shopItemElm;
};

const updateShoppingList = () => {
  const listElm = document.querySelector('#shopping-list');
  listElm.innerHTML = '';
  for (let i = 0; i < shoppingList.length; i += 1) {
    const itemElm = renderShopElm(shoppingList[i]);
    listElm.appendChild(itemElm);
  }
};
```

### Implementace klikání

Zatím jsme se nezabývali tím, jak implementovat kliknutí na položku seznamu tak, aby se nám ji povedlo vymazat. V podstatě jediný problém zde je, že posluchač události musí znát index položky, kterou chceme z pole smazat. Tento si však můžeme šikovně uložit do datového atributu například s názvem `data-index`. Pak už nám stačí pouze si tento index přečíst a smazat prvek pole pomocí metody `splice`. Pozor na to, že pak musíme zase zavolat funkci `updateShoppingList`, neboť jsme změnili obsah pole.

```js
const renderShopItem = (item, index) => {
  const shopItemElm = document.createElement('li');
  shopItemElm.className = 'shop-item';
  shopItemElm.dataset.index = index;
  shopItemElm.innerHTML = `<span>${item}</span><button>koupeno</button>`;

  const btnElm = shopItemElm.querySelector('button');
  btnElm.addEventListener('click', (e) => {
    const index = e.target.dataset.index;
    shoppingList.splice(index, 1);
    updateShoppingList();
  });

  return shopItemElm;
};
```

Funkce `renderShopItem` začiná být malinko dlouhá a trošku nepřehledná především kvůli tomu, že uvnitř ní vytváříme další funkci pro kliknutí na tlačítko. Nejlepší bude si tuto funkci pojmenovat a vytáhnout ven. Ḱód celé aplikace pak bude vypadat takto.

```js
'use strict';

const shoppingList = [
  'mrkev',
  'paprika',
  'cibule',
  'čínské zelí',
  'arašídy',
  'sojová omáčka',
];

const deleteClick = (e) => {
  const index = e.target.dataset.index;
  shoppingList.splice(index, 1);
  updateShoppingList();
};

const renderShopItem = (item, index) => {
  const shopItemElm = document.createElement('li');
  shopItemElm.className = 'shop-item';
  shopItemElm.dataset.index = index;
  shopItemElm.innerHTML = `<span>${item}</span><button>koupeno</button>`;

  const btnElm = shopItemElm.querySelector('button');
  btnElm.addEventListener('click', deleteClick);

  return shopItemElm;
};

const updateShoppingList = () => {
  const listElm = document.querySelector('#shopping-list');
  listElm.innerHTML = '';
  for (let i = 0; i < shoppingList.length; i += 1) {
    const itemElm = renderShopElm(shoppingList[i], i);
    listElm.appendChild(itemElm);
  }
};
```

@exercises ## Cvičení - vlastní DOM elementy [

- podcasty-2
  ]@
