# Recoil

## Documentation
https://recoiljs.org/

## Installation
```bash
# npmの場合
npm i recoil

# yarnの場合
yarn add recoil
```

## Usage
Rootの宣言
```javascript
/* src/App.tsx */

import { RecoilRoot } from "recoil";

function App() {
  return (
    <RecoilRoot>
      <Home />
    </RecoilRoot>
  );
}

export default App;
```

型定義
```javascript
/* src/types/User.ts */

export type User = {
  name: string;
  email: string;
  age: number;
};
```

Atom / Selectorを管理するキーファイルの作成
```typescript
/* src/grobalKeys/recoilKeys.ts */

export enum AtomKeys {
  USER_STATE = "userState",
  ARTICLE_LIST_STATE = "articleListState"
}

export enum SelectorKeys {
  USER_SELECTOR = "userSelector",
  ARTICLE_LIST_SELECTOR = "articleListSelector"
}
```

### Atomの作成
- データストア
- `key`が一意である必要がある
- `default`で初期値を指定
- `atom()`で宣言

```javascript
/* src/globalStates/atoms/userState.ts */

import { atom } from "recoil";

export const userStateAtom = atom({
  key: AtomKeys.USER_STATE,
  default: { isAdmin: false }
});
```

### Selectorの作成
- `Atom`の値を加工した結果を返す
- 副作用もここで処理する
- `Selector`内で扱っている`Atom`の値が更新されると再計算される
- `selector()`で宣言
- keyプロパティとgetプロパティは必須
- setプロパティはなくても良い

getメソッドはAtomから値を得る時の処理。
Selectorの中でAtomから値を得るには、getプロパティに渡される `get`関数を使用する

setメソッドはSelectorを通しての値の更新が可能になる。
setメソッドの第1引数には `get`関数と `set`関数、第2引数には更新したい値が渡される。

```javascript
/* src/globalStates/selector/userState.ts */

import { selector } from "recoil";

export const userStateSelector = selector({
  key: SelectorKeys.USER_SELECTOR,
  get: ({get}) => {
    const user = get(userStateAtom) // Atomの値を取得

    return user?.name.toUpperCase(); // 例：名前を大文字に変換して返す処理
  },
  set: ({set, get, reset}, value) => {
    console.log(value); // useRecoilStateなどのsetter関数の引数を取得(第二引数(value)の名前はなんでもOK）

    const user = get(userStateAtom) // Atomの値を取得
  }
});
```


### AtomやSelectorから値を取得する
値参照と更新を行いたい場合、useRecoilStateを使う。
```javascript
/* src/components/pages/Home.tsx */

import { useRecoilState } from "recoil";
import { userState } from "../../globalStates/atoms/userState";

export const Home = () => {
  const [userInfo, setUserInfo] = useRecoilState<User>(userState);
}
```

値参照のみ行いたい場合、useRecoilValueを使う。
```javascript
/* src/components/pages/User.tsx */

import { useRecoilValue } from "recoil";
import { userState } from "../../recoil/atoms/userState";

export const User = () => {
  const userInfo = useRecoilValue(userState);

  return (
  )
}
```

値更新のみ行いたい場合、useSetRecoilStateを使う。
```javascript
/* src/components/pages/Top.tsx */

import { useSetRecoilState } from "recoil"
import { userState } from "../../store/userState";

export const Top = () => {
  const setUserInfo = useSetRecoilState(userState);
}
```

### 非同期処理
