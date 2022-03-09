# MongoDB

## 1. Установка MongoDB

Установленная MongoDB без данных

![1.jpg](MongoDB_hw/1.jpg)

## 2. Заполнение данными

Создание новой бд и заполнение из csv файла

![2.jpg](MongoDB_hw/2.jpg)

Добавление одного объекта

```jsx
db.customers.insertOne(
	{
		"CustomerID" : 201,
		"Genre" : "Female",
		"Age": 22,
		"Annual Income (k$)": 14,
		"Spending Score (1-100)": 60
	}
)
{ acknowledged: true,
  insertedId: ObjectId("62290148ab26e157a8049f6e") }
```

Добавление нескольких объектов

```jsx
db.customers.insertMany([
	{
		"CustomerID" : 202,
		"Genre" : "Female",
		"Age": 25,
		"Annual Income (k$)": 20,
		"Spending Score (1-100)": 47
	}, 
	{
		"CustomerID" : 203,
		"Genre" : "Male",
		"Age": 40,
		"Annual Income (k$)": 11,
		"Spending Score (1-100)": 80
	}, 
	{
		"CustomerID" : 204,
		"Genre" : "Male",
		"Age": 35,
		"Annual Income (k$)": 37,
		"Spending Score (1-100)": 29
	}
])
{ acknowledged: true,
  insertedIds: 
   { '0': ObjectId("6229021fab26e157a8049f6f"),
     '1': ObjectId("6229021fab26e157a8049f70"),
     '2': ObjectId("6229021fab26e157a8049f71") } }
```

Добавленные нами вручную объекты:

![4.jpg](MongoDB_hw/4.jpg)

## 3. Выборка и обновление данных

Вывод пяти объектов отсортированных сначала по полю Age по возрастанию, затем по полю Annual Income (k$) по возрастанию

```jsx
db.customers.find({}, {_id: 0}).sort({Age: 1, "Annual Income (k$)": 1}).limit(5)
{ CustomerID: 34,
  Genre: 'Male',
  Age: 18,
  'Annual Income (k$)': 33,
  'Spending Score (1-100)': 92 }
{ CustomerID: 66,
  Genre: 'Male',
  Age: 18,
  'Annual Income (k$)': 48,
  'Spending Score (1-100)': 59 }
{ CustomerID: 92,
  Genre: 'Male',
  Age: 18,
  'Annual Income (k$)': 59,
  'Spending Score (1-100)': 41 }
{ CustomerID: 115,
  Genre: 'Female',
  Age: 18,
  'Annual Income (k$)': 65,
  'Spending Score (1-100)': 48 }
{ CustomerID: 1,
  Genre: 'Male',
  Age: 19,
  'Annual Income (k$)': 15,
  'Spending Score (1-100)': 39 }
```

Вывод пяти объектов с полем Age равным 22 или полем Annual Income (k$) меньшим 30

```jsx
db.customers.find({$or: [{Age: 22}, {"Annual Income (k$)": {$lt: 30}}]}, {_id: 0}).limit(5)
{ CustomerID: 1,
  Genre: 'Male',
  Age: 19,
  'Annual Income (k$)': 15,
  'Spending Score (1-100)': 39 }
{ CustomerID: 2,
  Genre: 'Male',
  Age: 21,
  'Annual Income (k$)': 15,
  'Spending Score (1-100)': 81 }
{ CustomerID: 3,
  Genre: 'Female',
  Age: 20,
  'Annual Income (k$)': 16,
  'Spending Score (1-100)': 6 }
{ CustomerID: 4,
  Genre: 'Female',
  Age: 23,
  'Annual Income (k$)': 16,
  'Spending Score (1-100)': 77 }
{ CustomerID: 5,
  Genre: 'Female',
  Age: 31,
  'Annual Income (k$)': 17,
  'Spending Score (1-100)': 40 }
```

Вывод шести объектов с полем  Age равным 23, или 24, или 25

```jsx
db.customers.find({Age: {$in: [23, 24, 25]}}, {_id: 0}).limit(6)
{ CustomerID: 4,
  Genre: 'Female',
  Age: 23,
  'Annual Income (k$)': 16,
  'Spending Score (1-100)': 77 }
{ CustomerID: 8,
  Genre: 'Female',
  Age: 23,
  'Annual Income (k$)': 18,
  'Spending Score (1-100)': 94 }
{ CustomerID: 14,
  Genre: 'Female',
  Age: 24,
  'Annual Income (k$)': 20,
  'Spending Score (1-100)': 77 }
{ CustomerID: 22,
  Genre: 'Male',
  Age: 25,
  'Annual Income (k$)': 24,
  'Spending Score (1-100)': 73 }
{ CustomerID: 30,
  Genre: 'Female',
  Age: 23,
  'Annual Income (k$)': 29,
  'Spending Score (1-100)': 87 }
{ CustomerID: 42,
  Genre: 'Male',
  Age: 24,
  'Annual Income (k$)': 38,
  'Spending Score (1-100)': 92 }
```

Обновление одного объекта с возраста 23 на возраст 24

```jsx
db.customers.updateOne({Age: 23}, {$set: {Age: 24}})
{ acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0 }
```

Обновление нескольких объектов с возрастом 27 на возраст 30

```jsx
db.customers.updateMany({Age: 27}, {$set: {Age: 30}})
{ acknowledged: true,
  insertedId: null,
  matchedCount: 6,
  modifiedCount: 6,
  upsertedCount: 0 }
```

Замена одного объекта но объект с другими полями

```jsx
db.customers.replaceOne(
	{Age: 35, Genre: "Male"},
	{Age: 33, Genre: "Female", "Annual Income (k$)": 55}
)
{ acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0 }
```

Удаление объектов с возрастом ≥ 18 и <20

```jsx
db.customers.deleteMany({Age: {$gte : 18}, Age: {$lt: 20}})
{ acknowledged: true, deletedCount: 12 }
```

Изменение, удаление и добавление объекта в одном запросе

```jsx
db.customers.bulkWrite([
	{
		updateOne: {
			"filter": {Age: 20},
			"update": {$set: {"Annual Income (k$)": 20}}
		}
	},
	{
		deleteOne: {
			"filter": {"Spending Score (1-100)": 99}
		}
	},
	{
		insertOne: {
			"document": {
				"CustomerID": 205,
				"Genre": "Female",
				"Age": 18,
				"Annual Income (k$)": 15,
				"Spending Score (1-100)": 67
			}
		}
	}
])
{ acknowledged: true,
  insertedCount: 1,
  insertedIds: { '2': ObjectId("62291758ab26e157a8049f7b") },
  matchedCount: 1,
  modifiedCount: 1,
  deletedCount: 1,
  upsertedCount: 0,
  upsertedIds: {} }
```

## 4. Индексы и сравнение производительности

Загрузили другой датасет с записями вопрос-ответ из телешоу

Без индексов время выполнения запроса 158ms

```jsx
db.questions.find({"Category String" : "TRAVEL & ROLES"}).explain('allPlansExecution')
{ explainVersion: '1',
  queryPlanner: 
   { namespace: 'new_database.questions',
     indexFilterSet: false,
     parsedQuery: { 'Category String': { '$eq': 'TRAVEL & ROLES' } },
     maxIndexedOrSolutionsReached: false,
     maxIndexedAndSolutionsReached: false,
     maxScansToExplodeReached: false,
     winningPlan: 
      { stage: 'COLLSCAN',
        filter: { 'Category String': { '$eq': 'TRAVEL & ROLES' } },
        direction: 'forward' },
     rejectedPlans: [] },
  executionStats: 
   { executionSuccess: true,
     nReturned: 0,
     executionTimeMillis: 158,
     totalKeysExamined: 0,
     totalDocsExamined: 216930,
     executionStages: 
      { stage: 'COLLSCAN',
        filter: { 'Category String': { '$eq': 'TRAVEL & ROLES' } },
        nReturned: 0,
        executionTimeMillisEstimate: 3,
        works: 216932,
        advanced: 0,
        needTime: 216931,
        needYield: 0,
        saveState: 216,
        restoreState: 216,
        isEOF: 1,
        direction: 'forward',
        docsExamined: 216930 },
     allPlansExecution: [] },
  command: 
   { find: 'questions',
     filter: { 'Category String': 'TRAVEL & ROLES' },
     '$db': 'new_database' },
  serverInfo: 
   { host: 'LILIA-W10',
     port: 27017,
     version: '5.0.6',
     gitVersion: '212a8dbb47f07427dae194a9c75baec1d81d9259' },
  serverParameters: 
   { internalQueryFacetBufferSizeBytes: 104857600,
     internalQueryFacetMaxOutputDocSizeBytes: 104857600,
     internalLookupStageIntermediateDocumentMaxSizeBytes: 104857600,
     internalDocumentSourceGroupMaxMemoryBytes: 104857600,
     internalQueryMaxBlockingSortMemoryUsageBytes: 104857600,
     internalQueryProhibitBlockingMergeOnMongoS: 0,
     internalQueryMaxAddToSetBytes: 104857600,
     internalDocumentSourceSetWindowFieldsMaxMemoryBytes: 104857600 },
  ok: 1 }
```

Создаем индексы

```jsx
db.questions.createIndex({"Category String" : 1})
'Category String_1'
```

С индексами выполнение такого же запроса 67ms

```jsx
db.questions.find({"Category String" : "TRAVEL & ROLES"}).explain('allPlansExecution')
{ explainVersion: '1',
  queryPlanner: 
   { namespace: 'new_database.questions',
     indexFilterSet: false,
     parsedQuery: { 'Category String': { '$eq': 'TRAVEL & ROLES' } },
     maxIndexedOrSolutionsReached: false,
     maxIndexedAndSolutionsReached: false,
     maxScansToExplodeReached: false,
     winningPlan: 
      { stage: 'FETCH',
        inputStage: 
         { stage: 'IXSCAN',
           keyPattern: { 'Category String': 1 },
           indexName: 'Category String_1',
           isMultiKey: false,
           multiKeyPaths: { 'Category String': [] },
           isUnique: false,
           isSparse: false,
           isPartial: false,
           indexVersion: 2,
           direction: 'forward',
           indexBounds: { 'Category String': [ '["TRAVEL & ROLES", "TRAVEL & ROLES"]' ] } } },
     rejectedPlans: [] },
  executionStats: 
   { executionSuccess: true,
     nReturned: 0,
     executionTimeMillis: 67,
     totalKeysExamined: 0,
     totalDocsExamined: 0,
     executionStages: 
      { stage: 'FETCH',
        nReturned: 0,
        executionTimeMillisEstimate: 67,
        works: 1,
        advanced: 0,
        needTime: 0,
        needYield: 0,
        saveState: 0,
        restoreState: 0,
        isEOF: 1,
        docsExamined: 0,
        alreadyHasObj: 0,
        inputStage: 
         { stage: 'IXSCAN',
           nReturned: 0,
           executionTimeMillisEstimate: 67,
           works: 1,
           advanced: 0,
           needTime: 0,
           needYield: 0,
           saveState: 0,
           restoreState: 0,
           isEOF: 1,
           keyPattern: { 'Category String': 1 },
           indexName: 'Category String_1',
           isMultiKey: false,
           multiKeyPaths: { 'Category String': [] },
           isUnique: false,
           isSparse: false,
           isPartial: false,
           indexVersion: 2,
           direction: 'forward',
           indexBounds: { 'Category String': [ '["TRAVEL & ROLES", "TRAVEL & ROLES"]' ] },
           keysExamined: 0,
           seeks: 1,
           dupsTested: 0,
           dupsDropped: 0 } },
     allPlansExecution: [] },
  command: 
   { find: 'questions',
     filter: { 'Category String': 'TRAVEL & ROLES' },
     '$db': 'new_database' },
  serverInfo: 
   { host: 'LILIA-W10',
     port: 27017,
     version: '5.0.6',
     gitVersion: '212a8dbb47f07427dae194a9c75baec1d81d9259' },
  serverParameters: 
   { internalQueryFacetBufferSizeBytes: 104857600,
     internalQueryFacetMaxOutputDocSizeBytes: 104857600,
     internalLookupStageIntermediateDocumentMaxSizeBytes: 104857600,
     internalDocumentSourceGroupMaxMemoryBytes: 104857600,
     internalQueryMaxBlockingSortMemoryUsageBytes: 104857600,
     internalQueryProhibitBlockingMergeOnMongoS: 0,
     internalQueryMaxAddToSetBytes: 104857600,
     internalDocumentSourceSetWindowFieldsMaxMemoryBytes: 104857600 },
  ok: 1 }
```