<template>
    <div>
        <div style="text-align: right">
            <Accessibility anchor="bottom-right"></Accessibility>
        </div>

        <h1>Lorem ipsum dolor sit amet.</h1>
        <h2>Lorem, ipsum dolor.</h2>
        <p>
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Reiciendis
            odit dolorum aperiam minus iure iste, odio ad accusantium eveniet,
            consequuntur explicabo, nisi inventore incidunt deleniti. Ex dolore
            aliquid dolor voluptatibus!
        </p>
        <p>
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Reiciendis
            odit dolorum aperiam minus iure iste, odio ad accusantium eveniet,
            consequuntur explicabo, nisi inventore incidunt deleniti. Ex dolore
            aliquid dolor voluptatibus!
        </p>
        <p>
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Reiciendis
            odit dolorum aperiam minus iure iste, odio ad accusantium eveniet,
            consequuntur explicabo, nisi inventore incidunt deleniti. Ex dolore
            aliquid dolor voluptatibus!
        </p>

        <nav>
            <ul>
                <li><a href="#">Item 1</a></li>
                <li><a href="#">Item 2</a></li>
                <li><a href="#">Item 3</a></li>
            </ul>
        </nav>
    </div>
</template>

<script>
import Accessibility from './components/Accessibility';
export default {
    name: 'App',
    components: {
        Accessibility,
    },
};
</script>
