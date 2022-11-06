# 立ち上げ

npm run dev

## page.tsx

app ディレクトリ以下の page.tsx の階層がパスになる。
/app/page.tsx → /
/app/hoge/page.tsx → /hoge
/app/[params]/page → /:params

## layout.tsx

配下の子コンポーネントで共通化されるレイアウト
つまり、/app/layout.tsx は /app/hoge/layout.tsx に反映される。
逆に /app/hoge/layout.tsx は /app/layout.tsx には反映されない。
また再レンダリングされないため、毎回実行したい処理がなるなどの場合は template.tsx を使う。

## template.tsx

配下の子コンポーネントで共通化されるレイアウトという点で、layout.tsx と似ているが、毎回新しくインスタンスを作成するという点で違いがある。
アニメーションや useEffect でのログ追跡などで必要な時に使用する。
また layout.tsx はサスペンスのフォールバックを最初のみ表示し、ページの切り替え時は表示しないのに対し、
template.tsx はページごとに表示するので、そういったケースでも使い分けることができる。

## Route Groups

ルーティングに影響を及ぼすことなく、関連するファイルをグルーピングするためのもの。
なので、以下のようになる。
/app/(main)/page.tsx → /
/app/(main)/hoge/page.tsx → /hoge
特定のレイアウトにしか使わない layout.tsx、template.tsx を別のコンポーネントに影響を与えたくない場合に使用する。
元々 page extensions のユースケースと同じようなもの。

## loading.tsx

同ディレクトリ以下のコンポーネント内でデータがまだ返って来ていない場合に表示されるコンポーネント。
Suspense を記述することなく、loading.tsx のみでローディング中のレイアウトを表示できる。
Promise を throw されているコンポーネントに対して、ローディング中のレイアウトを提供する。

## error.tsx

触ってみたけどよくわかっていない。普通にエラーが起きてしまう。
同階層のディレクトリ内のエラーバウンダリとして動作する？

## React をおさらい

### Server Components と Client Components

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

### Server Components のメリット

メリットとしては以下。

- バンドルサイズを減らせる
- データフェッチにかかる時間の減少
- Code Splitting の自動化
- レンダー結果の DOM への更新は最小限
