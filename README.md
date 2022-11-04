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
