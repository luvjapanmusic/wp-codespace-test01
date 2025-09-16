# ベースイメージ: PHP 8.4 + Apache + 開発用ツール
FROM mcr.microsoft.com/devcontainers/php:0-8.4

# 環境変数
ENV DEBIAN_FRONTEND=noninteractive

# 必要な OS パッケージをインストール
RUN apt-get update && apt-get install -y \
    git \
    unzip \
    curl \
    mariadb-client \
    less \
    && rm -rf /var/lib/apt/lists/*

# PHP拡張: WordPress に必要なもの
RUN docker-php-ext-install mysqli pdo pdo_mysql

# Composer をグローバルインストール（最新安定版）
RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer

# WP-CLI をグローバルインストール
RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
    && chmod +x wp-cli.phar \
    && mv wp-cli.phar /usr/local/bin/wp

# Node.js 最新 LTS（20系）をインストール（テーマ開発・Gutenberg対応）
ARG NODE_VERSION="20"
RUN if [ "${NODE_VERSION}" != "none" ]; then \
      su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} --lts && nvm alias default ${NODE_VERSION}" \
    ;fi

# Playwright（ブラウザ自動テスト用）の依存関係とブラウザをインストール
RUN yes | npx playwright install-deps
RUN npx playwright install

# 作業ディレクトリを設定
WORKDIR /var/www/html

# 推奨: 開発用に WP_DEBUG を有効化（docker-compose.yml で環境変数として渡すことも可能）
# ENV WP_DEBUG=1
