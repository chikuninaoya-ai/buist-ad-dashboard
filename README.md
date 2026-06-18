# ブイスト 広告ダッシュボード（Meta連携・単独版）

buist-kpi の「広告ダッシュボード」タブだけを単独アプリ化したもの。
**消化予算とキャンペーンの稼働状況(オンオフ)はMeta Graph APIからライブ取得**で自動更新。
登録/アポ/売上はGoogle Sheets（集計シート・プロマネシート）から取得。

## データソース
- 消化予算（日別・キャンペーン別）: **Meta Graph API**（`fetch_meta_daily_spend`）
- キャンペーン稼働状況 ACTIVE/PAUSED・日予算: **Meta Graph API**（`fetch_meta_campaign_status`）
- リスト登録・アポ・売上: Google Sheets（従来通り、サービスアカウント読み取り）

## デプロイ手順（Streamlit Community Cloud）
1. https://share.streamlit.io → New app
2. Repository: `chikuninaoya-ai/buist-ad-dashboard`、Branch: `main`、Main file: `app.py`
3. **Advanced settings → Secrets** に `.streamlit/secrets.toml` の中身をそのまま貼り付け
   - `[gcp_service_account]`（既存 buist-kpi と同じサービスアカウント）
   - `[meta]` access_token / ad_account_id / graph_version
4. Deploy

## ローカル実行
```
pip install -r requirements.txt
streamlit run app.py
```
`.streamlit/secrets.toml` が必要（gitignore済み・リポジトリには含めない）。

## 注意
- Meta access_token が失効するとMetaデータが取得できなくなります（長期トークン推奨）。
- Meta APIは ttl=600秒キャッシュ。最新化は画面下部「🔄 データを再読み込み」。
