# ModalBottomSheetの上にSnackBarを表示する

## 修正内容
通常、ModalBottomSheetでSnackBarを表示する場合、ModalBottomSheetの下にSnackbarが表示されてしまう。
そのためModalBottomSheetの上にSnackBarが表示されるように修正する。

### 修正前
```dart
class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: () {
        showModalBottomSheet(
          context: context,
          builder: (context) {
            return Center(
              child: ElevatedButton(
                onPressed: () {
                  ScaffoldMessenger.of(context).showSnackBar(
                    const SnackBar(content: Text('Hello!')),
                  );
                },
                child: const Text('Snack Bar'),
              ),
            );
          },
        );
      },
      child: const Text('Button'),
    );
  }
}
```


### 修正後
```dart
class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: () {
        showModalBottomSheet(
          context: context,
          builder: (context) {
            return Scaffold(
              body: Center(
                child: ElevatedButton(
                  onPressed: () {
                    ScaffoldMessenger.of(context).showSnackBar(
                      const SnackBar(content: Text('Hello!')),
                    );
                  },
                  child: const Text('Snack Bar'),
                ),
              ),
            );
          },
        );
      },
      child: const Text('Button'),
    );
  }
}
```

## サンプルコード
https://gist.github.com/hiromu-kon/3c5deb2ed06c755ac165304bf575c5b2


## DartPad
https://dartpad.dev/?id=3c5deb2ed06c755ac165304bf575c5b2
