# 立ち上げ

npm run dev

## page.tsx

app ディレクトリ以下の page.tsx の階層がパスになる。
/app/page.tsx → /
/app/hoge/page.tsx → /hoge

## layout.tsx

配下のディレクトリで共通化されるレイアウト
つまり、/app/layout.tsx は /app/hoge/layout.tsx に反映される。
逆に /app/hoge/layout.tsx は /app/layout.tsx には反映されない。

## Server Components と Client Components

Server Components は React コンポーネントをサーバーサイドでレンダリングすることができる技術。
SSR と違うのは、一部のコンポーネントのみをサーバーサイドでレンダリングするということ。
なので SSR のように、別のレンダリングが遅いコンポーネントに引っ張られることなく、効率的に画面を表示できる。
特徴としては、state を持てないので、useState を使えない。また再レンダリングが起こらないため、useEffect も使えない。
サーバー側でレンダリングされるため、ブラウザの API を使えず、シリアライズできないため、Client Components に関数を渡すこともできない。
Client Components は従来のクライアントでレンダリングされるコンポーネントと同様のもの。
state を持ったり、イベントハンドラ、Web API の使用などの必要がある場合はこちらを使う。
Client Components は Server Components を import することはできない。
これは Server Components のレンダリングに必要なサーバーへのリクエストのせいでパフォーマンスが低下してしまうことを防ぐため。
しかし、Client Components の children として Server Components を渡すことは可能。
基本的には Server Components を使い、state を持ちたかったり、イベントハンドラを使用したかったりする場合に Client Components を使う。

## Server Components のメリット

メリットとしては以下。

- バンドルサイズを減らせる
- データフェッチにかかる時間の減少
- Code Splitting の自動化
- レンダー結果の DOM への更新は最小限
