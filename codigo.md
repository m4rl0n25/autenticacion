Enter file contents here
from flask import Flask, render_template_string, request, redirect, session, url_for

app = Flask(__name__)
app.secret_key = 'mi_clave_secreta'

# Usuarios de ejemplo
usuarios = {
    "admin": "admin123",
    "usuario": "usuario123"
}

# Página de inicio
@app.route('/')
def inicio():
    if 'usuario' in session:
        return f"Hola, {session['usuario']}! <a href='/logout'>Cerrar sesión</a>"
    return redirect(url_for('login'))

# Página de login
@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        usuario = request.form['usuario']
        clave = request.form['clave']
        if usuario in usuarios and usuarios[usuario] == clave:
            session['usuario'] = usuario
            return redirect(url_for('inicio'))
        return "Usuario o contraseña incorrectos"
    return render_template_string('''
        <form method="post">
            Usuario: <input name="usuario"><br>
            Clave: <input type="password" name="clave"><br>
            <input type="submit" value="Ingresar">
        </form>
    ''')

# Cerrar sesión
@app.route('/logout')
def logout():
    session.pop('usuario', None)
    return redirect(url_for('login'))

if __name__ == '__main__':
    app.run(debug=True)
