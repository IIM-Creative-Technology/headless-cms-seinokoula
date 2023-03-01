<template>
    <header>
        <nav>
            <ul>
                <li><a href="/">Home</a></li>
            </ul>
        </nav>
    </header>
    <slot />
    <footer>
        <p>&copy; Alexandre CHARLES</p>
    </footer>
</template>

<style scoped>
header {
    background-color: #564138;
    color: white;
    padding: 10px;
    border-radius: 0 0 30px 30px;
    text-align: center;
    font-size: 1.4rem;
    font-weight: 700;
}

nav ul {
    list-style: none;
    margin: 0;
    padding: 0;;
}

nav li {
    display: inline-block;
    margin-right: 10px;
}

nav a {
    color: white;
    text-decoration: none;
}

main {
    padding: 20px;
}

footer {
    background-color: #eee;
    padding: 10px;
    bottom: 0;
    position: fixed;
    width: 100%;
    text-align: center;
}
</style>
