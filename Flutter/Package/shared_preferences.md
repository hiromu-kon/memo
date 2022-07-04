# shared_preferences

## Pub.dev
https://pub.dev/packages/shared_preferences

## Usage
Flutterでローカルにちょっとしたデータ保存したい場合にShared Preferencesというパッケージを利用する。

Shared preferencesで保存できるデータはキーバリュー形式のみ。

リストなども保存できる。

保存できるデータの型は以下

- int型
- double型
- bool型
- String型
- String型のリスト

### データの読み書き

**int型のデータを読み書きする**

```dart
// int型のデータを保存するにはsetIntを使用する
final SharedPreferences prefs = await SharedPreferences.getInstance();
prefs.setInt('counter', 123);

// int型のデータを取得するにはgetIntを使用する
final SharedPreferences prefs = await SharedPreferences.getInstance();
prefs.getInt('counter') ?? 0;
```

**double型のデータを読み書きする**
```dart
// double型のデータを保存するにはsetDoubleを使用する
final SharedPreferences prefs = await SharedPreferences.getInstance();
prefs.setDouble('counter_double', 123.00);

// double型のデータを取得するにはgetDoubleを使用する
final SharedPreferences prefs = await SharedPreferences.getInstance();
prefs.getDouble('counter_double') ?? 0.0;
```

**bool型のデータを読み書きする**
```dart
// bool型のデータを保存するにはsetBoolを使用する
final SharedPreferences prefs = await SharedPreferences.getInstance();
prefs.setBool('my_bool', true);

// bool型のデータを取得するにはgetBoolを使用する
final SharedPreferences prefs = await SharedPreferences.getInstance();
prefs.getBool('my_bool') ?? false;
```

**String型のデータを読み書きする**
```dart
// String型のデータを保存するにはsetStringを使用する
final SharedPreferences prefs = await SharedPreferences.getInstance();
prefs.setString('my_string', 'My String Data');

// String型のデータを取得するにはgetStringを使用する
final SharedPreferences prefs = await SharedPreferences.getInstance();
prefs.getString('my_string') ?? '';
```

**String型のリストを読み書きする**
```dart
// String型のリストを保存するにはsetStringListを使用する
final SharedPreferences prefs = await SharedPreferences.getInstance();
prefs.setStringList('my_string_list', ['apple', 'orange', 'grape']);

// String型のリストを取得するにはgetStringListを使用する
final SharedPreferences prefs = await SharedPreferences.getInstance();
prefs.getStringList('my_string_list') ?? [];
```

### データの削除

削除する場合は型に関わらず以下のようにキー名を指定して削除することができる
```dart
final SharedPreferences prefs = await SharedPreferences.getInstance();
prefs.remove('counter');
```
