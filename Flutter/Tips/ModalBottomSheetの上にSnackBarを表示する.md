# ModalBottomSheetの上にSnackBarを表示する

## 修正内容
通常、ModalBottomSheetでSnackBarを表示する場合、ModalBottomSheetの下にSnackbarが表示されてしまう。
そのためModalBottomSheetの上にSnackBarが表示されるように修正する。

### 修正前
https://user-images.githubusercontent.com/74192993/178089708-ad105b48-0c29-4d30-9cf5-c2a35c655af4.mov

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
https://user-images.githubusercontent.com/74192993/178089633-1a5b24a2-166f-4d68-b7e3-e257d0149a98.mov

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
