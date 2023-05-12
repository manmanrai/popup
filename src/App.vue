<template>
  <div id="app">
    <!-- <nav>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </nav> -->
    <!-- <router-view/> -->
    <div class="purchase">
      <button type="button" class="save_button" v-on:click="save">保存</button>
      <button type="button" class="reset_button" v-on:click="reset">リセット</button>
      <div v-if="total !== 0" class="purchase_detail">
        <br>
        <p>合計金額<br><b>{{ total.toLocaleString() }}円</b></p>
        <br>
        <p><b>内訳</b></p>
        <p v-if="detail.three">3,000円 {{ detail.three }}件</p>
        <p v-if="detail.four">4,000円 {{ detail.four }}件</p>
        <p v-if="detail.five">5,000円 {{ detail.five }}件</p>
      </div>
    </div>
    <section class="section">
      <p class="category_name"><b>キャリー</b> ¥5,000</p>
      <div class="category_flex">
        <template v-for="item in array">
          <dl
            v-if="item.category === 'carry'"
            class="product"
            :key="item.code"
            :class="[item.selected ? 'selected' : '']"
          >
            <dt :class="[item.sold === item.amount ? 'product_soldout': '']">
              <p class="product_name">{{ item.product }}</p>
              <p class="product_code">{{ item.code }}</p>
              <img v-if="item.img" :src="item.img" class="product_img" />
              <span v-if="item.size" class="product_size">{{ item.size }}</span>
            </dt>
            <dd>
              <p class="product_amount">
                <span class="product_sold">{{ item.sold }}</span>/{{ item.amount }}
              </p>
              <div class="product_table">
                <div class="product_cell">
                  <button type="button" v-on:click="minus(item.code)">-</button>
                </div>
                <div class="product_cell">
                  <button type="button" v-on:click="plus(item.code)">+</button>
                </div>
              </div>
            </dd>
          </dl>
        </template>
      </div>
    </section>
    <section class="section">
      <p class="category_name"><b>首輪 リード</b> ¥3,000</p>
      <div class="category_flex">
        <template v-for="item in array">
          <dl
            v-if="item.category === 'collar lead'"
            class="product"
            :key="item.code"
            :class="[item.selected ? 'selected' : '']"
          >
            <dt :class="[item.sold === item.amount ? 'product_soldout': '']">
              <p class="product_name">{{ item.product }}</p>
              <p class="product_code">{{ item.code }}</p>
              <img v-if="item.img" :src="item.img" class="product_img" />
              <span v-if="item.size" class="product_size">{{ item.size }}</span>
            </dt>
            <dd>
              <p class="product_amount">
                <span class="product_sold">{{ item.sold }}</span>/{{ item.amount }}
              </p>
              <div class="product_table">
                <div class="product_cell">
                  <button type="button" v-on:click="minus(item.code)">-</button>
                </div>
                <div class="product_cell">
                  <button type="button" v-on:click="plus(item.code)">+</button>
                </div>
              </div>
            </dd>
          </dl>
        </template>
      </div>
    </section>
    <section class="section">
      <p class="category_name"><b>ハーネス リード</b> ¥3,000</p>
      <div class="category_flex">
        <template v-for="item in array">
          <dl
            v-if="item.category === 'harness lead'"
            class="product"
            :key="item.code"
            :class="[item.selected ? 'selected' : '']"
          >
            <dt :class="[item.sold === item.amount ? 'product_soldout': '']">
              <p class="product_name">{{ item.product }}</p>
              <p class="product_code">{{ item.code }}</p>
              <img v-if="item.img" :src="item.img" class="product_img" />
              <span v-if="item.size" class="product_size">{{ item.size }}</span>
            </dt>
            <dd>
              <p class="product_amount">
                <span class="product_sold">{{ item.sold }}</span>/{{ item.amount }}
              </p>
              <div class="product_table">
                <div class="product_cell">
                  <button type="button" v-on:click="minus(item.code)">-</button>
                </div>
                <div class="product_cell">
                  <button type="button" v-on:click="plus(item.code)">+</button>
                </div>
              </div>
            </dd>
          </dl>
        </template>
      </div>
    </section>
    <section class="section">
      <p class="category_name"><b>ウェア</b> ¥4,000</p>
      <div class="category_flex">
        <template v-for="item in array">
          <dl
            v-if="item.category === 'wear'"
            class="product"
            :key="item.code"
            :class="[item.selected ? 'selected' : '']"
          >
            <dt :class="[item.sold === item.amount ? 'product_soldout': '']">
              <p class="product_name">{{ item.product }}</p>
              <p class="product_code">{{ item.code }}</p>
              <img v-if="item.img" :src="item.img" class="product_img" />
              <span v-if="item.size" class="product_size">{{ item.size }}</span>
            </dt>
            <dd>
              <p class="product_amount">
                <span class="product_sold">{{ item.sold }}</span>/{{ item.amount }}
              </p>
              <div class="product_table">
                <div class="product_cell">
                  <button type="button" v-on:click="minus(item.code)">-</button>
                </div>
                <div class="product_cell">
                  <button type="button" v-on:click="plus(item.code)">+</button>
                </div>
              </div>
            </dd>
          </dl>
        </template>
      </div>
    </section>
    <section v-if="purchase_history.length !== 0" class="section">
      <table class="history_table">
        <tbody>
          <tr>
            <td><b>購入時刻</b></td>
            <td><b>詳細</b></td>
            <td><b>合計金額</b></td>
          </tr>
          <tr v-for="history in purchase_history" :key="history.time">
            <td>{{ history.date }} {{ history.time }}</td>
            <td>
              <dl v-for="(item, index) in history.cart" :key="index">
                <dt><b>{{ item.product }}</b> {{ item.code }}</dt>
                <dd>{{ item.price }} {{ item.amount }}件</dd>
              </dl>
            </td>
            <td>{{ history.total }}</td>
          </tr>
        </tbody>
      </table>
      <button type="button" v-on:click="fullyReset()">テスト購入履歴を削除</button>
    </section>
  </div>
</template>

<script lang="ts">
  export default {
    data() {
      return {
        total: 0,
        detail: {
          three: 0,
          four: 0,
          five: 0
        },
        cart: [],
        purchase_history: [],
        array: [
          {
            category: "carry",
            product: "ライトナイロンキャリー-s-blue",
            code: "lf-c-nycarry-s-blue",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/nycarry-blue_1080x.png",
            amount: 20,
            price: "¥5,000",
            sold: 0,selected: false
        },
        {
            category: "carry",
            product: "ライトナイロンキャリー-m-blue",
            code: "lf-c-nycarry-m-blue",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/nycarry-blue_1080x.png",
            amount: 50,
            price: "¥5,000",
            sold: 0,selected: false
        },
        {
          category: "carry",
            product: "ライトナイロンキャリー-m-white",
            code: "lf-c-nycarry-m-white",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/nycarry-ecru_1080x.png",
            amount: 2,
            price: "¥5,000",
            sold: 0,selected: false
        },
        {
          category: "carry",
            product: "パイピングトラベルキャリー-beige",
            code: "lf-c-pitravel-beige",
            size: "",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/pitcarry01_1080x.png",
            amount: 2,
            price: "¥5,000",
            sold: 0,selected: false
        },
        {
            category: "carry",
            product: "旧citybackpackcarry",
            code: "lf-c-backpack-m-black-b",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/bpcarry-new01_1080x.png",
            amount: 9,
            price: "¥5,000",
            sold: 0,selected: false
        },
        {
            category: "carry",
            product: "旧citybackpackcarry",
            code: "lf-c-backpack-s-black-b",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/bpcarry-new01_1080x.png",
            amount: 44,
            price: "¥5,000",
            sold: 0,selected: false
        },
        {
            category: "carry",
            product: "ポータブルナイロンスリング-white",
            code: "lf-c-posling-white",
            size: "White",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/look01_ac209e03-2b99-43a2-9ea7-2fce6e89eebf_1296x.jpg",
            amount: 60,
            price: "¥5,000",
            sold: 0,selected: false
        },
        {
            category: "carry",
            product: "ポータブルナイロンスリング-black",
            code: "lf-c-posling-black",
            size: "Black",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/look01_ac209e03-2b99-43a2-9ea7-2fce6e89eebf_1296x.jpg",
            amount: 60,
            price: "¥5,000",
            sold: 0,selected: false
        },
        {
            category: "carry",
            product: "ポータブルナイロンスリング-khaki",
            code: "lf-c-posling-khaki",
            size: "Khaki",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/look01_ac209e03-2b99-43a2-9ea7-2fce6e89eebf_1296x.jpg",
            amount: 9,
            price: "¥5,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "シティロープカラー ホワイトM",
            code: "lf-c-ropecollar-m-white",
            size: "M",
            img: "https://shop.r10s.jp/4-l-f/cabinet/rope/collar/ropecollar-look08.jpg",
            amount: 32,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "シティロープカラー ホワイトL",
            code: "lf-c-ropecollar-l-white",
            size: "L",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/ropecollar01_1080x.png",
            amount: 22,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "シティロープカラー ブラックXS",
            code: "lf-c-ropecollar-xs-black",
            size: "XS",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/ropecollar-color-bk_1080x.png",
            amount: 18,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "シティロープカラー ブラックS",
            code: "lf-c-ropecollar-s-black",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/ropecollar-color-bk_1080x.png",
            amount: 14,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "シティロープカラー ブラックM",
            code: "lf-c-ropecollar-m-black",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/ropecollar-color-bk_1080x.png",
            amount: 11,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "シティロープカラー ブラックL",
            code: "lf-c-ropecollar-l-black",
            size: "L",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/ropecollar-color-bk_1080x.png",
            amount: 26,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "シティロープリード ホワイト",
            code: "lf-c-ropeleash-white",
            size: "",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/ropecollar-color-wh_76b2e2ff-09ab-42d8-95d2-241d0c399252_1080x.png",
            amount: 50,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "シティロープリード ブラック",
            code: "lf-c-ropeleash-black",
            size: "",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/ropecollar-color-bk_e5f0eb24-e82c-4721-9f3a-9bd666f9fb3c_1080x.png",
            amount: 50,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "ブライト首輪-s-orange",
            code: "lf-c-brcollar-s-org",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/brcollar-color-or_1080x.png",
            amount: 1,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "ブライト首輪-m-orange",
            code: "lf-c-brcollar-m-org",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/brcollar-color-or_1080x.png",
            amount: 21,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "ブライト首輪-s-yellow",
            code: "lf-c-brcollar-s-yel",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/brcollar-color-ye_1080x.png",
            amount: 33,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "ブライト首輪-m-yellow",
            code: "lf-c-brcollar-m-yel",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/brcollar-color-ye_1080x.png",
            amount: 59,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "ブライトハンズフリーリード-orange",
            code: "lf-c-brleash-org",
            size: "",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/brleash-color-or_1080x.png",
            amount: 28,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "ブライトハンズフリーリード-yellow",
            code: "lf-c-brleash-yel",
            size: "",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/brleash-color-ye_1080x.png",
            amount: 60,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "バイカラーシックー首輪-cs-xs-navy",
            code: "lf-c-bicollar-cs-xs-navy",
            size: "XS",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/bicollar-color_1080x.png",
            amount: 3,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "バイカラーシックー首輪-cs-s-navy",
            code: "lf-c-bicollar-cs-s-navy",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/bicollar-color_1080x.png",
            amount: 3,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "バイカラーシックー首輪-cs-m-navy",
            code: "lf-c-bicollar-cs-m-navy",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/bicollar-color_1080x.png",
            amount: 4,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "バイカラーシックー首輪-cs-l-navy",
            code: "lf-c-bicollar-cs-l-navy",
            size: "L",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/bicollar-color_1080x.png",
            amount: 21,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "バイカラーシックー首輪-cs-xs-grayge",
            code: "lf-c-bicollar-cs-xs-grayge",
            size: "XS",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/bicollar-color_1080x.png",
            amount: 11,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "バイカラーシックー首輪-cs-s-grayge",
            code: "lf-c-bicollar-cs-s-grayge",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/bicollar-color_1080x.png",
            amount: 5,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "バイカラーシックー首輪-cs-m-grayge",
            code: "lf-c-bicollar-cs-m-grayge",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/bicollar-color_1080x.png",
            amount: 1,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "バイカラーシックー首輪-cs-l-grayge",
            code: "lf-c-bicollar-cs-l-grayge",
            size: "L",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/bicollar-color_1080x.png",
            amount: 4,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "バイカラーシックリード-cs-sk-navy",
            code: "lf-c-bileash-cs-sk-navy",
            size: "Skinny",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/bicollar-leash-navy_1080x.png",
            amount: 25,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "バイカラーシックリード-cs-sk-grayge",
            code: "lf-c-bileash-cs-sk-grayge",
            size: "Skinny",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/bicollar-leash-grayge_1080x.png",
            amount: 20,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "バイカラーシックリード-cs-st-navy",
            code: "lf-c-bileash-cs-st-navy",
            size: "Standard",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/bicollar-leash-navy_1080x.png",
            amount: 16,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "バイカラーシックリード-cs-st-grayge",
            code: "lf-c-bileash-cs-st-grayge",
            size: "Standard",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/bicollar-leash-grayge_1080x.png",
            amount: 19,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "collar lead",
            product: "パイピングお散歩バッグ-brown",
            code: "lf-c-piwalkbag-brown",
            size: "",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/piwalkbag-br_1080x.png",
            amount: 8,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "ワンタッチキャンバスハーネス-s-beige",
            code: "lf-c-canharness-s-beige",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/canharness01_1080x.png",
            amount: 40,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "ワンタッチキャンバスハーネス-m-beige",
            code: "lf-c-canharness-m-beige",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/canharness01_1080x.png",
            amount: 40,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "ワンタッチキャンバスハーネス-s-black",
            code: "lf-c-canharness-s-black",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/canharness03_1080x.png",
            amount: 40,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "ワンタッチキャンバスハーネス-m-black",
            code: "lf-c-canharness-m-black",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/canharness03_1080x.png",
            amount: 40,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "ワンタッチキャンバスハーネス-s-green",
            code: "lf-c-canharness-s-green",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/canharness02_1080x.png",
            amount: 40,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "ワンタッチキャンバスハーネス-m-green",
            code: "lf-c-canharness-m-green",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/canharness02_1080x.png",
            amount: 40,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "サテンハーネス-s-pink",
            code: "lf-c-saharness-s-pink",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/saharness-pink_1080x.png",
            amount: 27,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "サテンハーネス-s-green",
            code: "lf-c-saharness-s-green",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/ssaharness-khaki_1080x.png",
            amount: 14,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "サテンハーネス-s-navy",
            code: "lf-c-saharness-s-navy",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/saharness-navy_1080x.png",
            amount: 12,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "サテンハーネス-m-green",
            code: "lf-c-saharness-m-green",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/ssaharness-khaki_1080x.png",
            amount: 3,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "サテンリード-pink-s",
            code: "lf-c-saleash-pink-s",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/saharness-pink_1080x.png",
            amount: 3,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "サテンリード-green-s",
            code: "lf-c-saleash-green-s",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/ssaharness-khaki_1080x.png",
            amount: 19,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "サテンリード-navy-s",
            code: "lf-c-saleash-navy-s",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/saharness-navy_1080x.png",
            amount: 18,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "サテンリード-pink-m",
            code: "lf-c-saleash-pink-m",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/saharness-pink_1080x.png",
            amount: 11,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "サテンリード-green-m",
            code: "lf-c-saleash-green-m",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/ssaharness-khaki_1080x.png",
            amount: 23,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "サテンリード-navy-m",
            code: "lf-c-saleash-navy-m",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/saharness-navy_1080x.png",
            amount: 19,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "トレンチハーネス-s-beige",
            code: "lf-c-trharness-s-beige",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/lf-c-trharness-6_1080x.jpg",
            amount: 40,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "トレンチハーネス-s-khaki",
            code: "lf-c-trharness-s-khaki",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/lf-c-trharness-5_1080x.jpg",
            amount: 40,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "トレンチハーネス-m-beige",
            code: "lf-c-trharness-m-beige",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/lf-c-trharness-6_1080x.jpg",
            amount: 40,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "トレンチハーネス-m-khaki",
            code: "lf-c-trharness-m-khaki",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/lf-c-trharness-5_1080x.jpg",
            amount: 40,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "トレンチハーネス-l-beige",
            code: "lf-c-trharness-l-beige",
            size: "L",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/lf-c-trharness-6_1080x.jpg",
            amount: 40,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "harness lead",
            product: "トレンチハーネス-l-khaki",
            code: "lf-c-trharness-l-khaki",
            size: "L",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/lf-c-trharness-5_1080x.jpg",
            amount: 40,
            price: "¥3,000",
            sold: 0,selected: false
        },
        {
            category: "wear",
            product: "キルトボアコート-s-green",
            code: "lf-c-quiltcoat-s-green",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/08_441bd069-9615-4a7f-8831-239a8f0bd621_1080x.jpg",
            amount: 40,
            price: "¥4,000",
            sold: 0,selected: false
        },
        {
            category: "wear",
            product: "キルトボアコート-s-brown",
            code: "lf-c-quiltcoat-s-brown",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/09_517f572a-38e1-4362-b381-84c3f2a6e7b6_1080x.jpg",
            amount: 30,
            price: "¥4,000",
            sold: 0,
            selected: false
        },
        {
            category: "wear",
            product: "キルトボアコート-m-green",
            code: "lf-c-quiltcoat-m-green",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/08_441bd069-9615-4a7f-8831-239a8f0bd621_1080x.jpg",
            amount: 40,
            price: "¥4,000",
            sold: 0,
            selected: false
        },
        {
            category: "wear",
            product: "キルトボアコート-m-brown",
            code: "lf-c-quiltcoat-m-brown",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/09_517f572a-38e1-4362-b381-84c3f2a6e7b6_1080x.jpg",
            amount: 40,
            price: "¥4,000",
            sold: 0,
            selected: false
        },
        {
            category: "wear",
            product: "キルトボアコート-l-green",
            code: "lf-c-quiltcoat-l-green",
            size: "L",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/08_441bd069-9615-4a7f-8831-239a8f0bd621_1080x.jpg",
            amount: 32,
            price: "¥4,000",
            sold: 0,
            selected: false
        },
        {
            category: "wear",
            product: "キルトボアコート-l-brown",
            code: "lf-c-quiltcoat-l-brown",
            size: "L",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/09_517f572a-38e1-4362-b381-84c3f2a6e7b6_1080x.jpg",
            amount: 25,
            price: "¥4,000",
            sold: 0,
            selected: false
        },
        {
            category: "wear",
            product: "CD-005ニットウェア-xs-ivory",
            code: "lf-c-knit-xs-ivory",
            size: "XS",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/04_16ea638a-2025-46c7-8209-1dab2ccf5b87_1080x.jpg",
            amount: 10,
            price: "¥4,000",
            sold: 0,
            selected: false
        },
        {
            category: "wear",
            product: "CD-005ニットウェア-xs-beige",
            code: "lf-c-knit-xs-beige",
            size: "XS",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/05_7310ae43-5ff2-4808-a0e3-556c243c371c_1080x.jpg",
            amount: 11,
            price: "¥4,000",
            sold: 0,
            selected: false
        },
        {
            category: "wear",
            product: "CD-005ニットウェア-xs-black",
            code: "lf-c-knit-xs-black",
            size: "XS",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/06_11631f3e-ea72-406a-9df7-1bd5e93be2ff_1080x.jpg",
            amount: 12,
            price: "¥4,000",
            sold: 0,
            selected: false
        },
        {
            category: "wear",
            product: "CD-005ニットウェア-s-ivory",
            code: "lf-c-knit-s-ivory",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/04_16ea638a-2025-46c7-8209-1dab2ccf5b87_1080x.jpg",
            amount: 4,
            price: "¥4,000",
            sold: 0,
            selected: false
        },
        {
            category: "wear",
            product: "CD-005ニットウェア-s-beige",
            code: "lf-c-knit-s-beige",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/05_7310ae43-5ff2-4808-a0e3-556c243c371c_1080x.jpg",
            amount: 12,
            price: "¥4,000",
            sold: 0,
            selected: false
        },
        {
            category: "wear",
            product: "CD-005ニットウェア-s-black",
            code: "lf-c-knit-s-black",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/06_11631f3e-ea72-406a-9df7-1bd5e93be2ff_1080x.jpg",
            amount: 13,
            price: "¥4,000",
            sold: 0,
            selected: false
        },
        {
            category: "wear",
            product: "CD-005ニットウェア-m-ivory",
            code: "lf-c-knit-m-ivory",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/04_16ea638a-2025-46c7-8209-1dab2ccf5b87_1080x.jpg",
            amount: 14,
            price: "¥4,000",
            sold: 0,
            selected: false
        },
        {
            category: "wear",
            product: "CD-005ニットウェア-m-beige",
            code: "lf-c-knit-m-beige",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/05_7310ae43-5ff2-4808-a0e3-556c243c371c_1080x.jpg",
            amount: 14,
            price: "¥4,000",
            sold: 0,
            selected: false
        },
        {
            category: "wear",
            product: "CD-005ニットウェア-m-black",
            code: "lf-c-knit-m-black",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/06_11631f3e-ea72-406a-9df7-1bd5e93be2ff_1080x.jpg",
            amount: 9,
            price: "¥4,000",
            sold: 0,
            selected: false
        },
        {
            category: "wear",
            product: "CD-003スウェット-xs-beige",
            code: "lf-c-sweat-xs-beige",
            size: "XS",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/03_d3f1c22a-efb8-43e7-aff4-31b19ac505e9_1080x.png",
            amount: 18,
            price: "¥4,000",
            sold: 0,
            selected: false
        },
        {
            category: "wear",
            product: "CD-003スウェット-xs-brown",
            code: "lf-c-sweat-xs-brown",
            size: "XS",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/04_5983b539-19b3-4290-8d81-769e1150f0f9_1080x.png",
            amount: 16,
            price: "¥4,000",
            sold: 0,
            selected: false
        },
        {
            category: "wear",
            product: "CD-003スウェット-s-beige",
            code: "lf-c-sweat-s-beige",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/03_d3f1c22a-efb8-43e7-aff4-31b19ac505e9_1080x.png",
            amount: 25,
            price: "¥4,000",
            sold: 0,
            selected: false
        },
        {
            category: "wear",
            product: "CD-003スウェット-s-brown",
            code: "lf-c-sweat-s-brown",
            size: "S",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/04_5983b539-19b3-4290-8d81-769e1150f0f9_1080x.png",
            amount: 32,
            price: "¥4,000",
            sold: 0,
            selected: false
        },
        {
            category: "wear",
            product: "CD-003スウェット-m-beige",
            code: "lf-c-sweat-m-beige",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/03_d3f1c22a-efb8-43e7-aff4-31b19ac505e9_1080x.png",
            amount: 16,
            price: "¥4,000",
            sold: 0,
            selected: false
        },
        {
            category: "wear",
            product: "CD-003スウェット-m-brown",
            code: "lf-c-sweat-m-brown",
            size: "M",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/04_5983b539-19b3-4290-8d81-769e1150f0f9_1080x.png",
            amount: 18,
            price: "¥4,000",
            sold: 0,
            selected: false
        },
        {
            category: "wear",
            product: "CD-003スウェット-l-beige",
            code: "lf-c-sweat-l-beige",
            size: "L",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/03_d3f1c22a-efb8-43e7-aff4-31b19ac505e9_1080x.png",
            amount: 19,
            price: "¥4,000",
            sold: 0,selected: false
        },
        {
            category: "wear",
            product: "CD-003スウェット-l-brown",
            code: "lf-c-sweat-l-brown",
            size: "L",
            img: "https://cdn.shopify.com/s/files/1/0022/2835/5129/products/04_5983b539-19b3-4290-8d81-769e1150f0f9_1080x.png",
            amount: 18,
            price: "¥4,000",
            sold: 0,selected: false
        }
        ]
      }
    },
    created() {
      if (localStorage.getItem('citydog_product_list') !== null) {
        this.array = JSON.parse(localStorage.getItem('citydog_product_list'))
      }
      if (localStorage.getItem('citydog_purchase_history') !== null) {
        this.purchase_history = JSON.parse(localStorage.getItem('citydog_purchase_history'))
      }
    },
    methods: {
      minus(code) {
        this.array.map((item) => {
          if (item.code === code) {
            item.sold = item.sold - 1
            item.selected = true
            const price = item.price.replace('¥', "").replace(',', "")
            this.total -= Number(price)
            if (price === '3000') {
              this.detail.three -= 1
            } else if (price === '4000') {
              this.detail.four -= 1
            } else if (price === '5000') {
              this.detail.five -= 1
            }

            const Obj = {
              product: item.product,
              code: item.code,
              price: item.price,
              amount: -1
            }
            if (this.cart.length === 0) {
              this.cart.push(Obj)
            } else {
              let exist = false
              this.cart.map((i) => {
                if (i.code === code) {
                  i.amount -= 1
                  exist = true
                }
              })
              if (!exist) {
                this.cart.push(Obj)
              }
            }
            // 数の調整ができるように、マイナスを許可する
          }
        })
      },
      plus(code) {
        this.array.map((item) => {
          if (item.code === code) {
            if (item.sold + 1 <= item.amount) {
              item.sold = item.sold + 1
              item.selected = true
              const price = item.price.replace('¥', "").replace(',', "")
              this.total += Number(price)
              if (price === '3000') {
                this.detail.three += 1
              } else if (price === '4000') {
                this.detail.four += 1
              } else if (price === '5000') {
                this.detail.five += 1
              }
              const addObj = {
                product: item.product,
                code: item.code,
                price: item.price,
                amount: 1
              }
              if (this.cart.length === 0) {
                this.cart.push(addObj)
              } else {
                let exist = false
                this.cart.map((i) => {
                  if (i.code === code) {
                    i.amount += 1
                    exist = true
                  }
                })
                if (!exist) {
                  this.cart.push(addObj)
                }
              }
            }
          }
        })
      },
      save() {
        this.array.map((item) => {
          item.selected = false
        })
        localStorage.setItem('citydog_product_list', JSON.stringify(this.array))

        
        const history = {
          date: new Date().toLocaleDateString(),
          time: new Date().toLocaleTimeString(),
          total: this.total,
          cart: this.cart
        }
        this.purchase_history.push(history)
        localStorage.setItem('citydog_purchase_history', JSON.stringify(this.purchase_history))

        // reset
        this.cart = []
        this.detail = {
          three: 0,
          four: 0,
          five: 0
        }
        this.total = 0
      },
      reset() {
        window.location.reload()
      },
      fullyReset() {
        localStorage.removeItem('citydog_product_list')
        localStorage.removeItem('citydog_purchase_history')
      }
    }
  }
</script>
<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
p,
dd {
  margin: 0;
}
.section {
  padding: 40px;
  border-bottom: 1px solid gray;
  width: 100%;
  max-width: 1360px;
  margin: 0 auto;
  box-sizing: border-box;
}
.category_name {
  background-color: #2c3e50;
  color: #fff;
  font-size: 22px;
  font-weight: bold;
  margin-bottom: 20px;
}
.category_flex {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
}
.product {
  display: block;
  margin: 10px;
  border: 4px solid transparent;
  padding: 20px;
  width: 250px;
  border-radius: 10px;
  background-color: #EFECEC;
}
.product.selected {
  border: 4px solid #FF4E00;
}
.product dt {
  position: relative;
}
.product_size {
  position: absolute;
  left: 10px;
  bottom: 10px;
  background-color: #861657;
  color: #fff;
  display: inline-block;
  padding: 0 10px;
  border-radius: 5px;
  font-size: 40px;
}
.product_name {
  font-size: 16px;
  font-weight: bold;
  height: 2.5rem;
  margin-bottom: 10px;
}
.product_code {
  margin-bottom: 5px;
}
.product_amount {
  font-size: 18px;
  font-weight: bold;
  color: #861657;
}
.product_sold {
  display: inline-block;
  margin-right: 5px;
  font-size: 20px;
  font-weight: bold;
  color: #A40606;
}
.product_img {
  width: 100%;
  height: auto;
}
.product_table {
  display: table;
  width: 100%;
  margin-top: 15px;
}
.product_cell {
  display: table-cell;
  width: 50%;
  vertical-align: top;
  padding: 0 5px;
}
.product_cell button {
  font-size: 22px;
  width: 100%;
  display: block;
  padding: 0 5px;
}
.product_soldout {
  opacity: 0.3;
  position: relative;
}
.product_soldout::after {
  content: 'SOLD OUT';
  position: absolute;
  left: 0;
  right: 0;
  width: 100%;
  font-size: 40px;
  font-weight: bold;
  top: 160px;
  text-align: center;
  z-index: 10;
}
.purchase {
  position: fixed;
  right: -4px;
  bottom: 100px;
  width: 80px;
  height: 250px;
}
.purchase button {
  display: block;
  width: 80px;
  height: 70px;
  line-height: 70px;
  font-size: 14px;
  text-align: center;
  border: 0;
}
.save_button {
  background-color: #0D324D;
  color: #fff;
}
.reset_button {
  background-color: #F53844;
  color: #fff;
}
.purchase_detail {
  font-size: 12px;
}
.history_table td {
  vertical-align: text-top;
  text-align: left;
  padding: 10px;
}
</style>
