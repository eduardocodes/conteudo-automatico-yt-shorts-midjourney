# 🎥 YouTube Automático Midjourney + LumaLabs

#### Vídeo:

## Fórmulas

### Tabela Filmes

```bash
lookup('Cenas', 'Vídeo')
```

### Tabela Cenas

```bash
concat(
    field('Prompt Imagem'),
    if(
        not(isblank(field('CREF'))),
        concat(' --cref ', field('CREF')),
        ''
    )
)
```

```bash
lookup('Filmes', 'Largura')
```

```bash
lookup('Filmes', 'Altura')
```

```bash
if(
    and(field('Altura') > 0, field('Largura') > 0),
    if(
        round(field('Largura') / field('Altura'), 2) = 1.33, '4:3',
        if(
            round(field('Largura') / field('Altura'), 2) = 1.78, '16:9',
            if(
                round(field('Largura') / field('Altura'), 2) = 0.56, '9:16',
                if(
                    round(field('Largura') / field('Altura'), 2) = 1.6, '16:10',
                    if(
                        round(field('Largura') / field('Altura'), 2) = 1.25, '5:4',
                        if(
                            round(field('Largura') / field('Altura'), 2) = 2.39, '21:9',
                            'Other'
                        )
                    )
                )
            )
        )
    ),
    'Invalid Dimensions'
)
```

## 👨‍💻 Códigos para geração das imagens no Midjourney

### Extrair IDs únicos sem ID Tarefa

```bash
const input = $input.all();

if (!input || input.length === 0) {
  return [];
}

const filteredObjects = input.filter(item => !item.json["ID Tarefa"]);

const ids = filteredObjects.map(item => item.json.id);

const uniqueIds = [...new Set(ids)];

return uniqueIds.map(id => ({ json: { id } }));
```

### Mesclar outputs por delay

```bash
const input = $input.all();

if (!input || input.length === 0) {
  return [];
}

const idList = input.filter(item => item.json.id !== undefined);
const taskList = input.filter(item => item.json.task_id !== undefined);

const minLength = Math.min(idList.length, taskList.length);

const pairedList = [];
for (let i = 0; i < minLength; i++) {
  pairedList.push({
    json: {
      id: idList[i].json.id,
      task_id: taskList[i].json.task_id,
      success: taskList[i].json.success,
      status: taskList[i].json.status,
      message: taskList[i].json.message
    }
  });
}

return pairedList;
```

### Extrair ID único sem Imagens

```bash
const input = $input.all();

if (!input || input.length === 0) {
  return [];
}

const filteredObjects = input.filter(item => !item.json.Imagens);

const ids = filteredObjects.map(item => item.json.id);

const uniqueIds = [...new Set(ids)];

return uniqueIds.map(id => ({ json: { id } }));
```

### Mesclar outputs por delay1

```bash
const input = $input.all();

if (!input || input.length === 0) {
  return [];
}

const idList = input.filter(item => item.json.id !== undefined);
const taskResultList = input.filter(item => item.json.task_id !== undefined);

const mergedList = idList.map((item, index) => {
  if (taskResultList[index] && taskResultList[index].json.task_result) {
    return {
      json: {
        id: item.json.id,
        image_url: taskResultList[index].json.task_result.image_url
      }
    };
  } else {
    return { json: { id: item.json.id, image_url: null } };
  }
});

return mergedList;
```

## 👨‍💻 Códigos para upscale de Imagens

### Extrair IDs únicos sem ID Tarefa Upscale

```bash
const input = $input.all();

if (!input || input.length === 0) {
  return [];
}

const filteredObjects = input.filter(item => !item.json["ID Tarefa Upscale"]);

const ids = filteredObjects.map(item => item.json.id);

const uniqueIds = [...new Set(ids)];

return uniqueIds.map(id => ({ json: { id } }));
```

### Mesclar outputs por delay2

```bash
const input = $input.all();

if (!input || input.length === 0) {
  return [];
}

const idList = input.filter(item => item.json.id !== undefined);
const taskResultList = input.filter(item => item.json.task_id !== undefined);

const mergedList = idList.map((item, index) => {
  if (taskResultList[index] && taskResultList[index].json.task_result) {
    return {
      json: {
        id: item.json.id,
        image_url: taskResultList[index].json.task_result.image_url
      }
    };
  } else {
    return { json: { id: item.json.id, image_url: null } };
  }
});

return mergedList;
```

### Extrair ID único sem Imagem Aumentada

```bash
const input = $input.all();

if (!input || input.length === 0) {
  return [];
}

const filteredObjects = input.filter(item => !item.json["Imagem Aumentada"]);

const ids = filteredObjects.map(item => item.json.id);

const uniqueIds = [...new Set(ids)];

return uniqueIds.map(id => ({ json: { id } }));
```

### Mesclar outputs por delay3

```bash
const input = $input.all();

if (!input || input.length === 0) {
  return [];
}

const idList = input.filter(item => item.json && item.json.id !== undefined && typeof item.json.id === 'number');

const videoUrls = Array.from(new Set(input
  .filter(item => item.json && item.json.data && item.json.data.generation && item.json.data.generation.video)
  .map(item => item.json.data.generation.video.url)
));

const mergedList = idList.map((item, index) => {
  return {
    json: {
      id: item.json.id,
      video_url: videoUrls[index] || null
    }
  };
});

return mergedList;
```

## 👨‍💻 Códigos para geração de cenas com LumaLabs

### Extrair ID único sem ID Tarefa LumaLabs

```bash
const input = $input.all();

if (!input || input.length === 0) {
  return [];
}

const filteredObjects = input.filter(item => !item.json["ID Tarefa LumaLabs"]);

const ids = filteredObjects.map(item => item.json.id);

const uniqueIds = [...new Set(ids)];

return uniqueIds.map(id => ({ json: { id } }));
```

### Mesclar outputs por delay4

```bash
const input = $input.all();

if (!input || input.length === 0) {
  return [];
}

const idList = input.filter(item => item.json.id !== undefined);
const taskList = input.filter(item => item.json.data && item.json.data.task_id !== undefined);

const minLength = Math.min(idList.length, taskList.length);

const pairedList = [];

for (let i = 0; i < minLength; i++) {
  pairedList.push({
    json: {
      id: idList[i].json.id,
      task_id: taskList[i].json.data.task_id,
      message: taskList[i].json.message,
      status: taskList[i].json.status || null,
      success: taskList[i].json.success || null
    }
  });
}

return pairedList;
```

### Extrair ID único sem Vídeo

```bash
const input = $input.all();

if (!input || input.length === 0) {
  return [];
}

const filteredObjects = input.filter(item => !item.json["Vídeo"]);

const ids = filteredObjects.map(item => item.json.id);

const uniqueIds = [...new Set(ids)];

return uniqueIds.map(id => ({ json: { id } }));
```

## Mesclar outputs por delay5

```bash
const input = $input.all();

if (!input || input.length === 0) {
  return [];
}

const idList = input.filter(item => item.json.id !== undefined);
const taskResultList = input.filter(item => item.json.task_id !== undefined);

const mergedList = idList.map((item, index) => {
  if (taskResultList[index] && taskResultList[index].json.task_result) {
    return {
      json: {
        id: item.json.id,
        image_url: taskResultList[index].json.task_result.image_url
      }
    };
  } else {
    return { json: { id: item.json.id, image_url: null } };
  }
});

return mergedList;
```

## 👨‍💻 Códigos para unir cenas

### Preparar input para JSON2Video

```bash
const inputData = items.map(item => item.json);

const item = inputData[0];

const videoUrls = item.Vídeo.map(video => video.value);

const scenes = videoUrls.map(url => ({
  elements: [
    {
      src: url,
      type: "video",
      zoom: 0,
      width: parseInt(item.Largura, 10),
      height: parseInt(item.Altura, 10),
      duration: -1,
      position: "center-center"
    }
  ]
}));

const singleMovieData = {
  resolution: "custom",
  quality: "high",
  width: parseInt(item.Largura, 10),
  height: parseInt(item.Altura, 10),
  scenes: scenes,
  elements: [],
  settings: {}
};

return [{ json: singleMovieData }];
```

### Extrair ID linha

```bash
const items = $input.all();

const ids = items.map(item => item.json.id);

const uniqueIds = [...new Set(ids)];

return uniqueIds.map(id => ({ json: { id } }));
```

### Mesclar outputs por delay do Merge2

```bash
function mergeOutputs(inputs) {
    let mergedOutput = [];
    let ids = inputs.filter(input => input.json.id !== undefined);
    let movies = inputs.filter(input => input.json.movie !== undefined);

    ids.forEach((id, index) => {
        if (index < movies.length) {
            mergedOutput.push({
                ...id.json,
                ...movies[index].json
            });
        } else {
            mergedOutput.push(id.json);
        }
    });

    movies.slice(ids.length).forEach(movie => {
        mergedOutput.push(movie.json);
    });

    return mergedOutput;
}

let inputs = $input.all();

if (!inputs || inputs.length === 0) {
    throw new Error("No inputs received.");
}

let output = mergeOutputs(inputs);

return output;
```

---

Made with 💙 by eduardocodes 👋
