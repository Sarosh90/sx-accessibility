<template>
    <div
        class="sxAccessibility"
        :class="[
            position ? 'sxAccessibility--' + position : '',
            anchor ? 'sxAccessibility--' + anchor : '',
            accessibilityVisible ? 'sxAccessibility--visible' : '',
        ]"
    >
        <div class="sxAccessibility__trigger" @click="toggle">
            <template v-if="$slots.trigger"
                ><slot name="trigger"></slot
            ></template>
            <template v-else
                ><button class="sxaBtn">
                    <i class="icon-sxa-wheelchair"></i></button
            ></template>
        </div>
        <div class="sxAccessibility__content">
            <button class="sxAccessibility__close" :style="[color ? {'backgroundColor':color} : '']" @click="toggle">Zapri</button>
            <ul v-if="accessibilityList">
                <li v-for="(item, index) in accessibilityList" :key="index">
                    <button
                        class="sxaBtn sxaBtn--block"
                        :class="{ 'is-active': item.active }"
                        @click="set(index)"
                    >
                        <i v-if="item.icon" :class="item.icon"></i>
                        <div v-html="item.title"></div>
                    </button>
                </li>
                <li>
                    <div>
                        <button
                            class="sxaBtn"
                            :class="{ 'is-active': item.active }"
                            @click="setFontSize(index)"
                            v-for="(item, index) in accessibilityFontList"
                            :key="index"
                        >
                            <div
                                v-html="item.title"
                                :class="item.spanClass"
                            ></div>
                        </button>
                    </div>
                </li>
            </ul>

            <button
                class="sxaBtn sxaBtn--sm sxaBtn--link"
                @click="removeAllCookies"
            >
                <span>Ponastavi nastavitve dostopnosti</span>
            </button>
        </div>
    </div>
</template>
<script>
export default {
    props: {
        position: {
            type: String,
            default: '', //absolute, fixed; default: absolute
        },
        anchor: {
            type: String,
            default: '', //top-left, top-right, bottom-left, bottom-right; default: top-right
        },
        color: {
            type: String,
            default:''
        }
    },
    data: () => ({
      cookieDuration:30,
        accessibilityVisible: false,
        accessibilityList: [
            {
                id: 'sxa-black-white',
                icon: 'icon-sxa-black-white',
                title: '??rno/belo',
                class: 'sx-accessibility--black-white',
                active: false,
                cookieValue: true,
            },
            {
                id: 'sxa-contrast',
                icon: 'icon-sxa-black',
                title: 'Temen kontrast',
                class: 'sx-accessibility--contrast',
                active: false,
                cookieValue: true,
            },
            {
                id: 'sxa-contrast-white',
                icon: 'icon-sxa-white',
                title: 'Svetel kontrast',
                class: 'sx-accessibility--contrast-white',
                active: false,
                cookieValue: true,
            },
            {
                id: 'sxa-animation-off',
                icon: 'icon-sxa-flash',
                title: 'Ustavi premike/animacije',
                class: 'sx-accessibility--animation-off',
                active: false,
                cookieValue: true,
            },
            {
                id: 'sxa-readable-font',
                icon: 'icon-sxa-font',
                title: 'Berljiva pisava',
                class: 'sx-accessibility--readable-font',
                active: false,
                cookieValue: true,
            },
            {
                id: 'sxa-underline-links',
                icon: 'icon-sxa-link',
                title: 'Pod??rtaj povezave',
                class: 'sx-accessibility--underline-links',
                active: false,
                cookieValue: true,
            },
        ],
        accessibilityFontList: [
            {
                id: 'sxa-font',
                title: 'A',
                class: 'sx-accessibility--font',
                spanClass: 'text-default',
                active: false,
                cookieValue: '1',
            },
            {
                id: 'sxa-font-1',
                title: 'A',
                class: 'sx-accessibility--font',
                spanClass: 'text-md',
                active: false,
                cookieValue: '1.1',
            },
            {
                id: 'sxa-font-2',
                title: 'A',
                class: 'sx-accessibility--font',
                spanClass: 'text-lg',
                active: false,
                cookieValue: '1.4',
            },
        ],
    }),
    mounted() {
        //Check If Accessibility Cookies Are Already Set
        this.getAccessibilities();
        this.getFontSizeAccessibilities();
    },
    methods: {
        //Toggle Accessibility Frame
        toggle() {
            this.accessibilityVisible = !this.accessibilityVisible;
        },
        //Set Accessibility
        set(index) {
            this.accessibilityList[index].active = !this.accessibilityList[index].active;
            this.accessibilityList[index].active ? this.setCookie(this.accessibilityList[index].id, this.accessibilityList[index].cookieValue, this.cookieDuration) : this.removeCookie(this.accessibilityList[index].id);
            this.getAccessibilities();
            this.toggle();
        },
        setFontSize(index) {
            this.accessibilityFontList[index].active = !this.accessibilityFontList[index].active;
            this.accessibilityFontList.map((e, i) => {
                if (index != i) e.active = false;
            });
            this.accessibilityFontList[index].active ? this.setCookie('sxa-font', this.accessibilityFontList[index].cookieValue,this.cookieDuration) : this.removeCookie('sxa-font');
            this.toggle();
            window.location.reload();
        },
        //Get Accessibilities
        getAccessibilities() {
            let body = document.body,
                externalCssA = ['sxa-contrast', 'sxa-contrast-white'];
            this.accessibilityList.map((item) => {
                if (this.getCookie(item.id)) {
                    item.active = true;
                    body.classList.add(item.class);
                    if (externalCssA.includes(item.id)) this.toggleAccessibilityCss(item.id, true);
                } else {
                    if (body.classList.contains(item.class)) body.classList.remove(item.class);
                    if (externalCssA.includes(item.id)) this.toggleAccessibilityCss(item.id, false);
                }
            });
        },
        getFontSizeAccessibilities() {
            let sxaFontCookie = this.getCookie('sxa-font');
            if (sxaFontCookie) {
                this.accessibilityFontList.map((item) => {
                    if (item.cookieValue == sxaFontCookie) item.active = true;
                });
                if (parseFloat(sxaFontCookie) > 1) {
                    this.changeTextSize(sxaFontCookie);
                }
            }
        },
        //Append Accessibility Css
        toggleAccessibilityCss(id, append) {
            if (append) {
                if (!document.getElementById(id)) {
                    let styleLink = document.createElement('link');
                    styleLink.setAttribute('rel', 'stylesheet');
                    styleLink.setAttribute('type', 'text/css');
                    styleLink.setAttribute('id', id);
                    styleLink.setAttribute('href', 'css/'+id + '.css');
                    document
                        .getElementsByTagName('head')[0]
                        .appendChild(styleLink);
                }
            } else {
                if (document.getElementById(id)) {
                    document.getElementById(id).remove();
                }
            }
        },
        //Alter Page Text Size
        changeTextSize(value) {
            if (value) {
                let textElements = document.querySelectorAll(
                    'p,span,a,h1,h2,h3,h4,h5,h6'
                );
                [].forEach.call(textElements, function (e) {
                    let currentFontSize = window.getComputedStyle(e).fontSize;

                    e.style.fontSize =
                        parseInt(currentFontSize) * parseFloat(value) + 'px';
                });
            }
        },
        //Remove All Accessibility Cookies
        removeAllCookies() {
            this.accessibilityList.map((item) => {if (item.active) this.removeCookie(item.id)});
            this.removeCookie('sxa-font');
            this.toggle();
            window.location.reload();
        },
        //Set Cookie
        setCookie(name, value, days) {
            let expires, date;
            if (days) {
                date = new Date();
                date.setTime(date.getTime() + days * 24 * 60 * 60 * 1000);
                expires = '; expires=' + date.toUTCString();
            }
            document.cookie = name + '=' + (value || '') + expires + '; path=/';
        },
        //Get Cookie
        getCookie(name) {
            var nameEQ = name + '=',
            ca = document.cookie.split(';');
            for (var i = 0; i < ca.length; i++) {
                var c = ca[i];
                while (c.charAt(0) == ' ') c = c.substring(1, c.length);
                if (c.indexOf(nameEQ) == 0)
                    return c.substring(nameEQ.length, c.length);
            }
            return null;
        },
        //Remove Cookie
        removeCookie(name) {
            document.cookie =
                name + '=; Path=/; Expires=Thu, 01 Jan 1970 00:00:01 GMT;';
        },
    },
};
</script>
