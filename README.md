# Menggunakan Redux untuk Mengelola State Global di React

Redux adalah pustaka pengelolaan state yang membantu mengelola state global dalam aplikasi React. Dalam panduan ini, kita akan membuat aplikasi counter sederhana yang menggunakan Redux untuk menyimpan dan mengelola state.

## 1. Mengonfigurasi Redux Store
Untuk menggunakan Redux di proyek React, instal redux dan react-redux menggunakan npm:
```bash
npm install redux react-redux
```
Buat file `store.js` untuk menginisialisasi store Redux menggunakan `createStore`.

### Kode: `store.js`

```javascript
import { createStore } from 'redux';
import counterReducer from './counterReducer';

// Membuat store Redux dengan counterReducer sebagai reducer
const store = createStore(counterReducer);

export default store;
```
Penjelasan
- createStore: Menggunakan createStore untuk membuat store Redux.
- counterReducer: counterReducer adalah reducer yang mengelola state count dalam aplikasi.
## 2. Membuat Reducer untuk Counter
Reducer adalah fungsi yang mengelola state berdasarkan jenis aksi (action) yang diterima. Buat file counterReducer.js sebagai reducer untuk count.

Kode: counterReducer.js
```javascript
// Initial state
const initialState = {
  count: 0
};

// Reducer
const counterReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return {
        ...state,
        count: state.count + 1
      };
    case 'DECREMENT':
      return {
        ...state,
        count: state.count - 1
      };
    default:
      return state;
  }
};

export default counterReducer;
```
### Penjelasan
- initialState: Mendefinisikan state awal dengan nilai count: 0.
- counterReducer: Fungsi reducer yang mengelola state count berdasarkan jenis aksi (INCREMENT dan DECREMENT).
    - INCREMENT: Menambah nilai count sebesar 1.
    - DECREMENT: Mengurangi nilai count sebesar 1.

## 3. Menghubungkan Redux Store dengan Aplikasi React
Untuk menghubungkan store Redux ke aplikasi React, kita gunakan Provider dari react-redux. Tambahkan kode berikut di file main.js atau index.js utama aplikasi.

Kode: main.js atau index.js
```javascript
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.jsx";
import store from "./redux/store";
import { Provider } from "react-redux";

ReactDOM.createRoot(document.getElementById("root")).render(
  <Provider store={store}>
    <React.StrictMode>
      <App />
    </React.StrictMode>
  </Provider>
);
```
Penjelasan
- Provider: Komponen Provider membungkus seluruh aplikasi React agar komponen dalam aplikasi dapat mengakses store Redux.
- store: Store yang telah kita buat (store.js) diimpor dan diberikan sebagai value di Provider.

## 4. Membuat Komponen Counter dengan Redux
Komponen Counter akan menampilkan nilai count dan memiliki tombol untuk menambah (INCREMENT) dan mengurangi (DECREMENT) nilai count. Komponen ini menggunakan useSelector dan useDispatch dari react-redux.

Kode: Counter.js
```javascript
import React from "react";
import { useSelector, useDispatch } from "react-redux";

function Counter() {
  // Mengambil state count dari redux
  const count = useSelector((state) => state.count);

  // Mendapatkan fungsi dispatch dari redux
  const dispatch = useDispatch();

  return (
    <div>
      <h1>Counter: {count}</h1>
      <button onClick={() => dispatch({ type: "INCREMENT" })}>Increment</button>
      <button onClick={() => dispatch({ type: "DECREMENT" })}>Decrement</button>
    </div>
  );
}

export default Counter;
```

Penjelasan
- useSelector: Hook useSelector digunakan untuk mengambil nilai count dari state Redux.
- useDispatch: Hook useDispatch digunakan untuk mengirimkan aksi (INCREMENT atau DECREMENT) ke store Redux.
- Aksi (Actions):
    - INCREMENT: Menambah count sebesar 1.
    - DECREMENT: Mengurangi count sebesar 1.

## Kesimpulan
Dengan konfigurasi ini, aplikasi React sekarang memiliki state global menggunakan Redux. Struktur aplikasi terdiri dari:
- Store (store.js): Menginisialisasi store Redux dengan reducer.
- Reducer (counterReducer.js): Mengelola state count berdasarkan aksi.
- Provider: Membungkus aplikasi untuk mengakses store Redux.
- Komponen Counter (Counter.js): Menampilkan nilai count dan memberikan kontrol untuk menambah atau mengurangi nilai.