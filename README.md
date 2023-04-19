# JWT-AUTH-ADA

const express = require('express');
const jwt = require('jsonwebtoken');
const bodyParser = require('body-parser');
const cors = require('cors');

const app = express();
const port = 3000;
const secretKey = 'ADA_SCHOOL';

// Middleware
app.use(bodyParser.json());
app.use(cors());

// Credenciales
const user = 'usuario';
const pass = 'abc123'


// Rutas
app.post('/login', (req, res) => {
  // Verificar las credenciales del usuario
  const username = req.body.user;
  const password = req.body.pass;

  if (username === user && password === pass) {
    // Crear el token JWT
    const payload = { username };
    const token = jwt.sign(payload, secretKey);
    // Enviar el token como respuesta
    res.send({ token });
  } else {
    res.status(401).send('Credenciales inválidas');
  }
});

app.get('/perfil', cerradura, (req, res) => {
    res.status(200).send(req.nombre.username)
})

//Esta función validará el token
function cerradura(req, res, next) {
    const authToken = req.headers.authorization;
    const jwToken = authToken && authToken.split(' ')[1];

    if (!jwToken) {
        res.status(400).send('Token invalido')
    }

    jwt.verify(jwToken, secretKey, (err, data) => {
        if (err) {
            res.status(400).send('Token invalido desde verificacion')
        }
        req.nombre = data
        next();
    });
}

// Iniciar el servidor
app.listen(port, () => {
  console.log(`Servidor iniciado en http://localhost:${port}`);
});
