# go_router

## Pub.dev
https://pub.dev/packages/go_router

Navigator 2.0 と呼ばれる宣言的な画面遷移を、簡単にするために作られたパッケージ。

## Usage

### Navigation
- Routerを定義する。

  go_routerを使わない場合はMaterialAppにhomeプロパティを追加する必要があるが、

  go_routerの場合は初期画面が`path: '/'`のページになるため設定不要。
```dart

final _router = GoRouter(
  routes: [
    GoRoute(
      path: '/',
      builder: (context, state) => const FirstPage(),
    ),
    GoRoute(
      name; 'page2' // nameを指定することができるが必須ではない
      path: '/page2',
      builder: (context, state) => const SecondPage(),
    ),
  ],
  // Errorページも宣言することができる
  errorBuilder: (context, state) => const ErrorPage(),
);

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {

    // MaterialAppをMatrialApp.routerに変更する
    return MaterialApp.router(
      // 下記二つを割り当てることで定義したrouteを読み込むことができる
      routerDelegate: _router.routerDelegate,
      routeInformationParser: _router.routeInformationParser
    );
  }
}
```

- 画面を遷移する。

  go_routerでの遷移はNavigatorを使わずにcontextを使う。
```dart
class FirstPage extends StatelessWidget {
  const FirstPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            // 遷移したいPathを指定して遷移することができる(前画面を破棄する)
            context.go('/page2');

            // goNamedでnameを指定して遷移することができる
            // 挙動はgoと同じ
            // context.goNamed('page2');

            // 遷移したいPathを指定して遷移することができる(前画面を破棄しない)
            // context.push('/page2');

            // pushNamedでnameを指定して遷移することができる
            // 挙動はpushと同じ
            // context.pushNamed('page2');
          },
          child: const Text('ページ2に遷移'),
        ),
      ),
    );
  }
}
```

- 遷移した画面から戻る。

```dart
class SecondPage extends StatelessWidget {
  const SecondPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            // popで遷移した画面から戻ることができる。
            context.pop();
          },
          child: const Text('戻る'),
        ),
      ),
    );
  }
}
```

### Sub routes
階層構造をrouteで作ることができる。
```dart
final _router = GoRouter(
  routes: [
    GoRoute(
      path: '/',
      builder: (context, state) => const FirstPage()
    ),
    GoRoute(
      name; 'page2'
      path: '/page2',
      builder: (context, state) => const SecondPage(),
      routes: [
        GoRoute(
          name; 'page3'
          path: '/page3',
          builder: (context, state) => const ThirdPage(),
        )
      ]
    ),
  ]
);
```

```dart
class FirstPage extends StatelessWidget {
  const FirstPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            // ページ1の上にページ3が乗っかってる状態
            context.push('/page2/page3');

            // ページ2の上にページ３が乗っかってる状態
            // context.go('/page2/page3');

            // nameを指定した場合
            // context.pushNamed('page3');
            // context.goNamed('page3');
          },
          child: const Text('ページ3に遷移'),
        ),
      ),
    );
  }
}

class SecondPage extends StatelessWidget {
  const SecondPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            context.pop();
          },
          child: const Text('戻る'),
        ),
      ),
    );
  }
}

class ThirdPage extends StatelessWidget {
  const ThirdPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            context.pop();
          },
          child: const Text('戻る'),
        ),
      ),
    );
  }
}
```

### Parameters
パラメータを付与することができる。
```dart
final _router = GoRouter(
  routes: [
    GoRoute(
      path: '/',
      builder: (context, state) => const FirstPage(),
    ),
    GoRoute(
      name; 'page2'
      path: '/page2/:id',
      builder: (context, state) {
        // パラメータを取得し送る
        String? id = state.params['id'];
        return SecondPage(id: id);
      },
    ),
  ]
);
```

```dart
class FirstPage extends StatelessWidget {
  const FirstPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            // パラメータを送る
            // パラメータがなかった場合、エラーが発生する(Errorページに遷移)
            context.go('/page2/1234');
          },
          child: const Text('ページ2に遷移'),
        ),
      ),
    );
  }
}

class SecondPage extends StatelessWidget {
  const SecondPage({Key? key, this.id}) : super(key: key);

  // コンストラクタ経由でパラメータを受け取る
  final String? id;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Column(
          children: [
            Text(id ?? 'idはありません'),
            ElevatedButton(
              onPressed: () {
                context.pop();
              },
              child: const Text('戻る'),
            ),
          ],
        ),
      ),
    );
  }
}
```

### Redirection
条件によってページ遷移をリダイレクトできる。
```dart
final _router = GoRouter(
  routes: [
    GoRoute(
      path: '/',
      builder: (context, state) => const FirstPage(),
    ),
    GoRoute(
      name; 'page2'
      path: '/page2/:id',
      builder: (context, state) {
        // パラメータを取得し送る
        String? id = state.params['id'];
        return SecondPage(id: id);
      },
    ),
    GoRoute(
      name; 'redirect'
      path: '/redirect',
      builder: (context, state) => const RedirectPage(),
    ),
  ],
  redirect: (state) {
    const isLoggedIn = false;

    if (!isLoggedIn && state.logation == '/page2') {
      return '/redirect';
    }

    // nullを返すことでRedirectせずに通常の画面に遷移することができる
    return null;
  }
);
```
