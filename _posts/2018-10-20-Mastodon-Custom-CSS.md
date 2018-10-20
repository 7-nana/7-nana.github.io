---
layout: post
comments: true
title: カスタムCSS機能を利用してMastodonの既存テーマをカスタマイズする
date: 2018-10-20 00:00:00
image: https://7-nana.github.io/images/2018-10-20-Mastodon-Custom-CSS-002.jpg
categories: Mastodon
tags:
- CSS
---

Mastodonのv2.5.0から、管理画面に「カスタムCSS」という機能が追加されました。これにより、既存テーマのCSSを管理画面側からカスタマイズできるようになります。

SCSSによるテーマ追加が思うようにできなかった方や、Mastodonのホスティングサービス（[Masto.host](https://masto.host/)や[Hostodon](https://hostdon.jp/)）を利用している方には、とても便利な機能です。

## カスタムCSSの入力欄

カスタムCSSの入力欄は、管理画面の「管理」＜「サイト設定」の下部にあります。すぐ上に「カスタム詳細説明」や「カスタム利用規約」の入力欄があるので、誤って上書きしてしまわないように注意しましょう。

![カスタムCSSの入力欄](https://7-nana.github.io/images/2018-10-20-Mastodon-Custom-CSS-001.jpg)

## 既存テーマの確認

Mastodonのテーマは「Mastodon」「Mastodon（ライト）」「ハイコントラスト」の３種類あります。

「Mastodon」が紺ベースのダークテーマ、「Mastodon（ライト）」は白ベースのライトテーマ、「ハイコントラスト」は全体が黒っぽく、ボタンなどがより明るい色に設定されています。

Web UIとパブリックページ（Aboutや公開アカウントのプロフィールなど）では、bodyタグに各テーマ固有のクラス名が割り振られています。

「Mastodon」なら「theme-default」、「Mastodon（ライト）」なら「theme-mastodon-light」、「ハイコントラスト」なら「theme-contrast」です。

これらのクラス名を利用することで、個別のテーマに対しCSSを上書きすることができます。逆に言えば、クラス名を利用せずCSSを上書きすると、３つのテーマ全てにスタイルが適用されるということです。

例えば、

```
/* 検索窓の角を丸くする */
.search__input,
.status__content .status__content__spoiler-link {
   border-radius:25px;
}
```

上記のCSSは３つ全てのテーマに適用されます。
そして、

```
/* 検索窓の角を丸くする */
body.theme-default .search__input,
body.theme-default .status__content .status__content__spoiler-link {
   border-radius:25px;
}
```

上記のCSSは「Mastodon」テーマにのみ適用されます。

## 管理画面のCSSについて

残念なことに、管理画面のbodyタグには各テーマ固有のクラス名が無く、カスタマイズしにくい状態です。

できなくはないのですが、全てのテーマの管理画面を変更してしまうため、ダークテーマでもライトテーマでも問題なく視認できるか、確認しながら作業する必要があります。これは大変手間がかかります。

管理画面もカスタマイズしたい場合は、テーマファイル一式を用意して追加する、従来の方法をとった方が良いと思います。

## 「ハイコントラスト」テーマをカスタマイズする

私は「Mastodon」と「Mastodon（ライト）」はあまり触らずに置いておき、「ハイコントラスト」を大幅にカスタマイズすることにしました。

テーマ名は変えられないので、ユーザーが困惑しないよう、テーマ名の通り「ハイコントラスト（背景と文字がはっきり区別できる状態）」であることを維持します。また変更箇所を最小限に抑えるため、テーマの明るいところ（文字）と暗いところ（背景）も維持します。

### カスタマイズ後の状態

最終的に、[このような見栄えになりました](https://mastodos.com/)。紫色をベースにしたのは、京都府の紋章に紫が使用されているからです。

![Aboutページ](https://7-nana.github.io/images/2018-10-20-Mastodon-Custom-CSS-002.jpg)

![このインスタンスについて](https://7-nana.github.io/images/2018-10-20-Mastodon-Custom-CSS-003.jpg)

![Web UI](https://7-nana.github.io/images/2018-10-20-Mastodon-Custom-CSS-004.jpg)

### 作成したカスタムCSS

私が自分のインスタンスに適用したカスタムCSSは以下のとおりです。
※Mastodon v2.5.2で確認

興味がある方はコピペして使ってみてください。このCSSはMastodonのライセンス（AGPL-3.0）同様、どなたでも自由に使用することができます。

また、（私は使用していないので未検証ですが）もしかするとユーザースタイルシートにも流用できるかもしれません。

```
/* ----------------------------------------------
   ハイコントラストテーマに適用
---------------------------------------------- */

/* 全体（一番下）とカラム設定の背景色を変える */
body.theme-contrast,
body.theme-contrast .column-header__collapsible-inner,
body.theme-contrast .column-header__button.active,
body.theme-contrast .ui {
   background-color: #3d3059;
}

/* カラムヘッダーの背景色を変える */
body.theme-contrast a.column-link,
body.theme-contrast .column-back-button,
body.theme-contrast .column-header,
body.theme-contrast .column-header__button,
body.theme-contrast .column-header__back-button,
body.theme-contrast .drawer__header,
body.theme-contrast .list-editor h4,
body.theme-contrast .public-layout .header,
body.theme-contrast .search-results__header,
body.theme-contrast .search-results__section h5,
body.theme-contrast .tabs-bar {
   background-color: #7760a9;
}

/* カラムの背景色を変える */
body.theme-contrast .activity-stream .entry,
body.theme-contrast .account__header__fields dt,
body.theme-contrast .box-widget,
body.theme-contrast .card__bar,
body.theme-contrast .card__img,
body.theme-contrast .column-link__badge,
body.theme-contrast .column-subheading,
body.theme-contrast .column-inline-form,
body.theme-contrast .column>.scrollable,
body.theme-contrast .contact-widget,
body.theme-contrast .detailed-status,
body.theme-contrast .detailed-status__action-bar,
body.theme-contrast .drawer__inner,
body.theme-contrast .drawer__inner__mastodon,
body.theme-contrast .empty-column-indicator,
body.theme-contrast .error-column,
body.theme-contrast .flex-spacer,
body.theme-contrast .getting-started,
body.theme-contrast .getting-started__wrapper,
body.theme-contrast .hero-widget__text,
body.theme-contrast .landing-page__information.contact-widget,
body.theme-contrast .landing-page #mastodon-timeline,
body.theme-contrast .landing-page__forms,
body.theme-contrast .landing-page__information,
body.theme-contrast .landing-page__call-to-action,
body.theme-contrast .landing-page__information:last-child,
body.theme-contrast .landing-page .separator-or span,
body.theme-contrast .list-editor,
body.theme-contrast .public-layout .public-account-header__bar:before,
body.theme-contrast .public-layout .public-account-bio,
body.theme-contrast .search__input,
body.theme-contrast .status.status-direct,
body.theme-contrast .status-card .status-card__content {
   background-color: #5f4b8b;
}

/* アカウントプロフィールのヘッダーと背景色を変える */
body.theme-contrast .account__section-headline,
body.theme-contrast .account__header .account__header__fields dt,
body.theme-contrast .account__header .account__header__fields dd,
body.theme-contrast .media-spoiler {
   background-color: #48396a;
}

/* アカウントプロフィールのヘッダー画像に被せる色を変える */
body.theme-contrast .account__header>div {
    background: rgba(61,48,89,.9);
}

/* リプライ時の返信先トゥートの背景色を変える */
body.theme-contrast .reply-indicator {
   background-color: #ebebeb;
}

/* ドロップダウンメニューの背景色を変える */
body.theme-contrast .dropdown-menu,
body.theme-contrast .dropdown-menu__item a {
   background-color: #fff;
}
body.theme-contrast .dropdown-menu__arrow.bottom {
   border-bottom-color: #fff;
}

/* メインの文字色を変える */
body.theme-contrast,
body.theme-contrast a.drawer__tab,
body.theme-contrast .account__header .account__header__fields dt,
body.theme-contrast .account__section-headline a.active,
body.theme-contrast .account__header__fields dt,
body.theme-contrast .column-header>.column-header__back-button,
body.theme-contrast .column-header__back-button,
body.theme-contrast .column-back-button,
body.theme-contrast .column-header__button,
body.theme-contrast .column-header__button.active,
body.theme-contrast .column-settings__section,
body.theme-contrast .column-header__collapsible,
body.theme-contrast .dropdown-menu__item a:active,
body.theme-contrast .dropdown-menu__item a:focus,
body.theme-contrast .dropdown-menu__item a:hover,
body.theme-contrast .hero-widget__text,
body.theme-contrast .landing-page li,
body.theme-contrast .landing-page p,
body.theme-contrast .notification__message,
body.theme-contrast .public-layout .header .nav-link,
body.theme-contrast .rich-formatting h3,
body.theme-contrast .rich-formatting h4,
body.theme-contrast .rich-formatting li,
body.theme-contrast .rich-formatting p,
body.theme-contrast .setting-meta__label,
body.theme-contrast .setting-toggle__label,
body.theme-contrast .simple_form p.hint.subtle-hint,
body.theme-contrast .status-card:hover .status-card__content,
body.theme-contrast .status-card:hover .status-card__title,
body.theme-contrast .status-card:hover .status-card__description {
    color: #fff;
}

/* 通知、その他の文字色を変える */
body.theme-contrast .account__relationship .icon-button,
body.theme-contrast .account__section-headline a,
body.theme-contrast .account__action-bar__tab>span,
body.theme-contrast .detailed-status__meta,
body.theme-contrast .getting-started__footer p,
body.theme-contrast .muted .attachment-list__list a,
body.theme-contrast .muted .status__content a,
body.theme-contrast .muted .status__content p,
body.theme-contrast .muted .status__display-name strong,
body.theme-contrast .public-layout .public-account-bio .roles,
body.theme-contrast .public-layout .public-account-bio__extra,
body.theme-contrast .status__relative-time,
body.theme-contrast .status__display-name,
body.theme-contrast .status-card,
body.theme-contrast .status-card__title,
body.theme-contrast .status-card__description,
body.theme-contrast .status__prepend,
body.theme-contrast .status__prepend .status__display-name strong {
   color: #d2cbe3;
}

/* リンクの文字色を変える */
body.theme-contrast .account__relationship .icon-button.active,
body.theme-contrast .account__header .account__header__username,
body.theme-contrast .account__header__fields a,
body.theme-contrast .attachment-list__list a,
body.theme-contrast .status__content a,
body.theme-contrast .column-header.active .column-header__icon,
body.theme-contrast .compose__action-bar-dropdown .icon-button.active,
body.theme-contrast .getting-started__footer a,
body.theme-contrast .notification__message .fa,
body.theme-contrast .public-layout .public-account-bio .account__header__fields a,
body.theme-contrast .reply-indicator__content a,
body.theme-contrast .rich-formatting li a,
body.theme-contrast .rich-formatting p a,
body.theme-contrast .simple_form p.hint.subtle-hint a,
body.theme-contrast .tabs-bar__link.active,
body.theme-contrast .text-icon-button.active {
   color: #64bf2f;
}

/* 「もっと見る」の文字色を変える */
body.theme-contrast .reply-indicator__content .status__content__spoiler-link,
body.theme-contrast .status__content .status__content__spoiler-link {
   color: #5f4b8b;
}

/* トゥート下などのアクションバーの文字色を変える */
body.theme-contrast .account__action-bar-dropdown .icon-button,
body.theme-contrast .detailed-status__button .icon-button,
body.theme-contrast .detailed-status__action-bar-dropdown .icon-button,
body.theme-contrast .icon-button.disabled,
body.theme-contrast .notification-favourite .status.status-direct .icon-button.disabled,
body.theme-contrast .status__action-bar__counter .icon-button,
body.theme-contrast .status__action-bar__counter__label,
body.theme-contrast .status__action-bar .icon-button {
   color: #917eba;
}

/* 入力欄のプレースホルダーの文字色を変える */
body.theme-contrast .search__input:placeholder-shown,
body.theme-contrast .setting-text:placeholder-shown,
body.theme-contrast .required:placeholder-shown {
   color: #9c8bc1;
}
body.theme-contrast .search__input::-webkit-input-placeholder,
body.theme-contrast .setting-text::-webkit-input-placeholder,
body.theme-contrast .required::-webkit-input-placeholder {
   color: #9c8bc1;
}
body.theme-contrast .search__input:-ms-input-placeholder,
body.theme-contrast .setting-text:-ms-input-placeholder,
body.theme-contrast .required:-ms-input-placeholder {
   color: #9c8bc1;
}

/* ボタンの背景色を変える */
body.theme-contrast a.column-link:hover,
body.theme-contrast .button,
body.theme-contrast .dropdown-menu__item a:active,
body.theme-contrast .dropdown-menu__item a:focus,
body.theme-contrast .dropdown-menu__item a:hover,
body.theme-contrast .floating-action-button,
body.theme-contrast .public-layout .header .nav-button,
body.theme-contrast .react-toggle--checked .react-toggle-track,
body.theme-contrast .reply-indicator__content .status__content__spoiler-link,
body.theme-contrast .status__content .status__content__spoiler-link,
body.theme-contrast .status-card:hover .status-card__content,
body.theme-contrast .simple_form .block-button,
body.theme-contrast .simple_form .button,
body.theme-contrast .simple_form button {
   background-color: #64bf2f;
}
body.theme-contrast .react-toggle--checked .react-toggle-thumb {
   border-color: #64bf2f;
}
body.theme-contrast .public-layout .public-account-header__tabs__tabs .counter.active:after {
   border-bottom: 4px solid #64bf2f;
}

/* ボタンのホバー・フォーカス時の背景色を変える */
body.theme-contrast a.drawer__tab:hover,
body.theme-contrast .card>a:hover .card__bar,
body.theme-contrast .column-header__button:hover,
body.theme-contrast .dropdown-menu__item a:hover,
body.theme-contrast .public-layout .header .brand:hover,
body.theme-contrast .privacy-dropdown.active .privacy-dropdown__value.active,
body.theme-contrast .privacy-dropdown__option.active,
body.theme-contrast .privacy-dropdown__option:hover,
body.theme-contrast .simple_form .block-button:hover,
body.theme-contrast .simple_form .button:hover,
body.theme-contrast .simple_form button:hover {
   background-color: #41b200;
}

/* トグルスイッチのオフ時の色を変える */
body.theme-contrast .react-toggle-track {
   background-color: #999;
}
body.theme-contrast .react-toggle-thumb {
   border: 1px solid #999;
}

/* リスト名入力欄の下線の色を変える */
body.theme-contrast .column-inline-form label input:focus {
   border-bottom: 2px solid #41b200;
}

/* スマホ版タブバーのアクティブ時のボーダーの色を変える */
body.theme-contrast .tabs-bar__link.active {
    border-bottom: 2px solid #64bf2f;
}

/* プロフィールページのアバター画像のボーダーと背景色を無くす */
body.theme-contrast .public-layout .public-account-header__bar .avatar img {
    border: none;
    background: transparent;
}

/* 各種ボーダーの微調整 */
body.theme-contrast .account__header__fields,
body.theme-contrast .account__header .account__header__fields dl,
body.theme-contrast .detailed-status__action-bar {
   border-top: 1px solid #48396a;
}
body.theme-contrast .public-layout .public-account-header__tabs__tabs .counter:last-child {
   border-right: none;
}
body.theme-contrast .account__action-bar__tab,
body.theme-contrast .public-layout .public-account-header__tabs__tabs .counter {
   border-right: 1px solid #48396a;
}
body.theme-contrast .account__section-headline,
body.theme-contrast .detailed-status__action-bar,
body.theme-contrast .status {
   border-bottom: 1px solid #48396a;
}
body.theme-contrast .status-card {
   border: 1px solid #48396a;
}
body.theme-contrast .account__section-headline a.active:after {
   border-color: transparent transparent #5f4b8b;
}

/* 「お気に入り」したあとのアイコンの色を変える */
body.theme-contrast .notification__favourite-icon-wrapper .star-icon,
body.theme-contrast .star-icon.active {
    color: #eca72c;
}

/* ブーストする前・したあとのアイコンの色を変える */
body.theme-contrast button.icon-button i.fa-retweet {
    background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='22' height='209'><path d='M4.97 3.16c-.1.03-.17.1-.22.18L.8 8.24c-.2.3.03.78.4.8H3.6v2.68c0 4.26-.55 3.62 3.66 3.62h7.66l-2.3-2.84c-.03-.02-.03-.04-.05-.06H7.27c-.44 0-.72-.3-.72-.72v-2.7h2.5c.37.03.63-.48.4-.77L5.5 3.35c-.12-.17-.34-.25-.53-.2zm12.16.43c-.55-.02-1.32.02-2.4.02H7.1l2.32 2.85.03.06h5.25c.42 0 .72.28.72.72v2.7h-2.5c-.36.02-.56.54-.3.8l3.92 4.9c.18.25.6.25.78 0l3.94-4.9c.26-.28 0-.83-.37-.8H18.4v-2.7c0-3.15.4-3.62-1.25-3.66z' fill='%23917eba' stroke-width='0'/><path d='M7.78 19.66c-.24.02-.44.25-.44.5v2.46h-.06c-1.08 0-1.86-.03-2.4-.03-1.64 0-1.25.43-1.25 3.65v4.47c0 4.26-.56 3.62 3.65 3.62H8.5l-1.3-1.06c-.1-.08-.18-.2-.2-.3-.02-.17.06-.35.2-.45l1.33-1.1H7.28c-.44 0-.72-.3-.72-.7v-4.48c0-.44.28-.72.72-.72h.06v2.5c0 .38.54.63.82.38l4.9-3.93c.25-.18.25-.6 0-.78l-4.9-3.92c-.1-.1-.24-.14-.38-.12zm9.34 2.93c-.54-.02-1.3.02-2.4.02h-1.25l1.3 1.07c.1.07.18.2.2.33.02.16-.06.3-.2.4l-1.33 1.1h1.28c.42 0 .72.28.72.72v4.47c0 .42-.3.72-.72.72h-.1v-2.47c0-.3-.3-.53-.6-.47-.07 0-.14.05-.2.1l-4.9 3.93c-.26.18-.26.6 0 .78l4.9 3.92c.27.25.82 0 .8-.38v-2.5h.1c4.27 0 3.65.67 3.65-3.62v-4.47c0-3.15.4-3.62-1.25-3.66zM10.34 38.66c-.24.02-.44.25-.43.5v2.47H7.3c-1.08 0-1.86-.04-2.4-.04-1.64 0-1.25.43-1.25 3.65v4.47c0 3.66-.23 3.7 2.34 3.66l-1.34-1.1c-.1-.08-.18-.2-.2-.3 0-.17.07-.35.2-.45l1.96-1.6c-.03-.06-.04-.13-.04-.2v-4.48c0-.44.28-.72.72-.72H9.9v2.5c0 .36.5.6.8.38l4.93-3.93c.24-.18.24-.6 0-.78l-4.94-3.92c-.1-.08-.23-.13-.36-.12zm5.63 2.93l1.34 1.1c.1.07.18.2.2.33.02.16-.03.3-.16.4l-1.96 1.6c.02.07.06.13.06.22v4.47c0 .42-.3.72-.72.72h-2.66v-2.47c0-.3-.3-.53-.6-.47-.06.02-.12.05-.18.1l-4.94 3.93c-.24.18-.24.6 0 .78l4.94 3.92c.28.22.78-.02.78-.38v-2.5h2.66c4.27 0 3.65.67 3.65-3.62v-4.47c0-3.66.34-3.7-2.4-3.66zM13.06 57.66c-.23.03-.4.26-.4.5v2.47H7.28c-1.08 0-1.86-.04-2.4-.04-1.64 0-1.25.43-1.25 3.65v4.87l2.93-2.37v-2.5c0-.44.28-.72.72-.72h5.38v2.5c0 .36.5.6.78.38l4.94-3.93c.24-.18.24-.6 0-.78l-4.94-3.92c-.1-.1-.24-.14-.38-.12zm5.3 6.15l-2.92 2.4v2.52c0 .42-.3.72-.72.72h-5.4v-2.47c0-.3-.32-.53-.6-.47-.07.02-.13.05-.2.1L3.6 70.52c-.25.18-.25.6 0 .78l4.93 3.92c.28.22.78-.02.78-.38v-2.5h5.42c4.27 0 3.65.67 3.65-3.62v-4.47-.44zM19.25 78.8c-.1.03-.2.1-.28.17l-.9.9c-.44-.3-1.36-.25-3.35-.25H7.28c-1.08 0-1.86-.03-2.4-.03-1.64 0-1.25.43-1.25 3.65v.7l2.93.3v-1c0-.44.28-.72.72-.72h7.44c.2 0 .37.08.5.2l-1.8 1.8c-.25.26-.08.76.27.8l6.27.7c.28.03.56-.25.53-.53l-.7-6.25c0-.27-.3-.48-.55-.44zm-17.2 6.1c-.2.07-.36.3-.33.54l.7 6.25c.02.36.58.55.83.27l.8-.8c.02 0 .04-.02.04 0 .46.24 1.37.17 3.18.17h7.44c4.27 0 3.65.67 3.65-3.62v-.75l-2.93-.3v1.05c0 .42-.3.72-.72.72H7.28c-.15 0-.3-.03-.4-.1L8.8 86.4c.3-.24.1-.8-.27-.84l-6.28-.65h-.2zM4.88 98.6c-1.33 0-1.34.48-1.3 2.3l1.14-1.37c.08-.1.22-.17.34-.2.16 0 .34.08.44.2l1.66 2.03c.04 0 .07-.03.12-.03h7.44c.34 0 .57.2.65.5h-2.43c-.34.05-.53.52-.3.78l3.92 4.95c.18.24.6.24.78 0l3.94-4.94c.22-.27-.02-.76-.37-.77H18.4c.02-3.9.6-3.4-3.66-3.4H7.28c-1.08 0-1.86-.04-2.4-.04zm.15 2.46c-.1.03-.2.1-.28.2l-3.94 4.9c-.2.28.03.77.4.78H3.6c-.02 3.94-.45 3.4 3.66 3.4h7.44c3.65 0 3.74.3 3.7-2.25l-1.1 1.34c-.1.1-.2.17-.32.2-.16 0-.34-.08-.44-.2l-1.65-2.03c-.06.02-.1.04-.18.04H7.28c-.35 0-.57-.2-.66-.5h2.44c.37 0 .63-.5.4-.78l-3.96-4.9c-.1-.15-.3-.23-.47-.2zM4.88 117.6c-1.16 0-1.3.3-1.3 1.56l1.14-1.38c.08-.1.22-.14.34-.16.16 0 .34.04.44.16l2.22 2.75h7c.42 0 .72.28.72.72v.53h-2.6c-.3.1-.43.54-.2.78l3.92 4.9c.18.25.6.25.78 0l3.94-4.9c.22-.28-.02-.77-.37-.78H18.4v-.53c0-4.2.72-3.63-3.66-3.63H7.28c-1.08 0-1.86-.03-2.4-.03zm.1 1.74c-.1.03-.17.1-.23.16L.8 124.44c-.2.28.03.77.4.78H3.6v.5c0 4.26-.55 3.62 3.66 3.62h7.44c1.03 0 1.74.02 2.28 0-.16.02-.34-.03-.44-.15l-2.22-2.76H7.28c-.44 0-.72-.3-.72-.72v-.5h2.5c.37.02.63-.5.4-.78L5.5 119.5c-.12-.15-.34-.22-.53-.16zm12.02 10c1.2-.02 1.4-.25 1.4-1.53l-1.1 1.36c-.07.1-.17.17-.3.18zM5.94 136.6l2.37 2.93h6.42c.42 0 .72.28.72.72v1.25h-2.6c-.3.1-.43.54-.2.78l3.92 4.9c.18.25.6.25.78 0l3.94-4.9c.22-.28-.02-.77-.37-.78H18.4v-1.25c0-4.2.72-3.63-3.66-3.63H7.28c-.6 0-.92-.02-1.34-.03zm-1.72.06c-.4.08-.54.3-.6.75l.6-.74zm.84.93c-.12 0-.24.08-.3.18l-3.95 4.9c-.24.3 0 .83.4.82H3.6v1.22c0 4.26-.55 3.62 3.66 3.62h7.44c.63 0 .97.02 1.4.03l-2.37-2.93H7.28c-.44 0-.72-.3-.72-.72v-1.22h2.5c.4.04.67-.53.4-.8l-3.96-4.92c-.1-.13-.27-.2-.44-.2zm13.28 10.03l-.56.7c.36-.07.5-.3.56-.7zM17.13 155.6c-.55-.02-1.32.03-2.4.03h-8.2l2.38 2.9h5.82c.42 0 .72.28.72.72v1.97H12.9c-.32.06-.48.52-.28.78l3.94 4.94c.2.23.6.22.78-.03l3.94-4.9c.22-.28-.02-.77-.37-.78H18.4v-1.97c0-3.15.4-3.62-1.25-3.66zm-12.1.28c-.1.02-.2.1-.28.18l-3.94 4.9c-.2.3.03.78.4.8H3.6v1.96c0 4.26-.55 3.62 3.66 3.62h8.24l-2.36-2.9H7.28c-.44 0-.72-.3-.72-.72v-1.97h2.5c.37.02.63-.5.4-.78l-3.96-4.9c-.1-.15-.3-.22-.47-.2zM5.13 174.5c-.15 0-.3.07-.38.2L.8 179.6c-.24.27 0 .82.4.8H3.6v2.32c0 4.26-.55 3.62 3.66 3.62h7.94l-2.35-2.9h-5.6c-.43 0-.7-.3-.7-.72v-2.3h2.5c.38.03.66-.54.4-.83l-3.97-4.9c-.1-.13-.23-.2-.38-.2zm12 .1c-.55-.02-1.32.03-2.4.03H6.83l2.35 2.9h5.52c.42 0 .72.28.72.72v2.34h-2.6c-.3.1-.43.53-.2.78l3.92 4.9c.18.24.6.24.78 0l3.94-4.9c.22-.3-.02-.78-.37-.8H18.4v-2.33c0-3.15.4-3.62-1.25-3.66zM4.97 193.16c-.1.03-.17.1-.22.18l-3.94 4.9c-.2.3.03.78.4.8H3.6v2.68c0 4.26-.55 3.62 3.66 3.62h7.66l-2.3-2.84c-.03-.02-.03-.04-.05-.06H7.27c-.44 0-.72-.3-.72-.72v-2.7h2.5c.37.03.63-.48.4-.77l-3.96-4.9c-.12-.17-.34-.25-.53-.2zm12.16.43c-.55-.02-1.32.03-2.4.03H7.1l2.32 2.84.03.06h5.25c.42 0 .72.28.72.72v2.7h-2.5c-.36.02-.56.54-.3.8l3.92 4.9c.18.25.6.25.78 0l3.94-4.9c.26-.28 0-.83-.37-.8H18.4v-2.7c0-3.15.4-3.62-1.25-3.66z' fill='%2364bf2f' stroke-width='0'/></svg>");
}
```

### カスタムCSSの解説

便宜上、変更する箇所はなるべく最小限にし、スタイル別にまとめています。コメントで大まかにどこがどう変わるのか書いているので、カラーコードをお好みのものに一括置換すればサイト全体の色合いをガラリと変えることができます。

恐らく一番やっかいと思われるのがsvgでアニメーションする「お気に入り」アイコンです。ここはソースが長いので一番下に記述しています。

一見複雑なように見えますが、「お気に入り」アイコンの色を変えるだけなら、「fill=」の部分だけを見ればOKです。

「body.theme-contrast button.icon-button i.fa-retweet {」の中に、「fill=」は２箇所あります。ファイル内検索で探してみてください。上記のソースだと、「fill='%23917eba'」と「fill='%2364bf2f'」がそれです。

ここはベクターデータを「何色で塗りつぶす（fill）か」を指定しています。シングルクォーテーションで囲っている部分、先頭の「%23」はエンコードした「#」なので、そのうしろの文字列が16進数のカラーコードというわけです。

１つ目の「fill=」がお気に入りする前のアイコンの色、２つ目の「fill=」がお気に入りしたあとのアイコンの色を指定しています。この２箇所を変更するだけで、アニメーションを保持しつつお気に入りアイコンの色を変更することができます。

### カスタムCSSを作成したあとの作業

ひと通りのカスタマイズが済んだら、「管理」＜「サイト設定」の「Theme」で「ハイコントラスト」を選択し保存します。

これにより、ログインしていないユーザーや、ユーザー設定でテーマを選択していないユーザーに対し、「ハイコントラスト」テーマが適用されるようになります（既存ユーザーに対しては一度案内をした方が良いでしょう）。

## 「ハイコントラスト」テーマ以外のカスタマイズ

私のインスタンスでは、上記の他に、以下のCSSも適用しています。
※Mastodon v2.5.2で確認

```
/* ----------------------------------------------
   全テーマと管理画面に適用
---------------------------------------------- */

/* 各種ボタンと検索窓の角を丸くする */
.button,
.landing-page__forms .button,
.public-layout .header .nav-button,
.reply-indicator__content .status__content__spoiler-link,
.search__input,
.status__content .status__content__spoiler-link,
.simple_form button,
.simple_form .button.negative {
   border-radius:25px;
}


/* ----------------------------------------------
   ライトテーマに適用
---------------------------------------------- */

/* カラムヘッダーを白くする */
body.theme-mastodon-light .column-header,
body.theme-mastodon-light .column-header__button,
body.theme-mastodon-light .column-back-button,
body.theme-mastodon-light .column-header__back-button,
body.theme-mastodon-light .drawer__header {
   background-color: #fff;
}
body.theme-mastodon-light .column-header,
body.theme-mastodon-light .column-back-button {
   border-bottom: 1px solid #c0cdd9;
}

/* カラムのスクロールバーに背景色を付ける */
body.theme-mastodon-light .column>.scrollable {
   background-color: #d9e1e8;
}
body.theme-mastodon-light .detailed-status,
body.theme-mastodon-light .detailed-status__action-bar,
body.theme-mastodon-light .item-list {
   background-color: #fff;
}
```

## 機能が増え続けるMastodon

以上のように、Mastodon v2.5.0からは、ホスティングサービスを利用しているインスタンスでも見た目の個性が出せるようになりました。こういったカスタマイズは、ブログのそれと同じでとても楽しいものですし、ワクワクしますよね。

あとは、管理画面左上のMastodonロゴマークをオリジナルのものに変更できて、自動的にfaviconやスマホのホームアイコンに反映されたら良いのにな〜、なんて思ってます（他力本願）。

色はその場所のムードを表現します。自分に合ったインスタンスを探しているユーザーにとっては、判断基準のひとつになることもあるでしょう。ぜひインスタンスの趣旨や集まるクラスタに合わせた、最適な色合いを模索してみてください。[Quesdon](https://quesdon.rinsuki.net/)や[Votedon](https://vote.thedesk.top/)で意見を募りながら進めていくのも面白いかもしれませんね。

カスタムCSSを活用することで、これまでテーマを追加できなかった管理者さんやそのユーザーの皆さんが、インスタンスに対し更に愛着を持つことができたら良いなと思います。

この記事が、どなたかの一助になれば幸いです。
