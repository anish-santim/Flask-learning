# Sample code to run flask app
from flask import Flask 
app = Flask(__name__)

@app.route('/')
def hello():
    return "<h1>Hello Flask!</h1>"

# steps to run this flask app
1. python3 -m venv venv
2. source venv/bin/activate
3. pip3 install flask
4. export FLASK_DEBUG=True 
5. flask run 

# Routes in flask 
@app.route('/')
@app.route('/home')
def home():
    return "<h1>Home Page</h1>"

# Render templates(html files in templates folder) in flask 
from flask import Flask, render_template
@app.route('/home')
def home():
    return render_template('home.html')

# Jinja Template
// app.py 
def home():
    return render_template('home.html', posts=posts)

// home.html
{% if posts %}
    {% for post in posts %}
        <h1>{{ post.title }}</h1>
        <p>By {{ post.author }} on {{ post.content }}</p>
    {% endfor %}
{% else %} 
    <h1>No such posts</h1>
{% endif %}

# Template Inheritance
// base.html

<html>
<head></head>
<body>
    {% block content %}{% endblock %}
</body>
</html>

// home.html

{% extends "base.html" %}
{% block content %}
    <h1>Home Page</h1>
{% endblock %}

# Add static files(css/js in static folder)
from flask import Flask, render_template, url_for

//base.html
<link rel="stylesheet" type="text/css" href="{{ url_for('static', filename='main.css') }}">

# Flask forms

// forms.py
from wsgiref.validate import validator
from flask_wtf import FlaskForm 
from wtforms import StringField, PasswordField, SubmitField, BooleanField
from wtforms.validators import DataRequired, Length, Email, EqualTo

class Form(FlaskForm):
    name = StringField(Nname', validators=[DataRequired(), Length(min=2, max=20)])
    submit = SubmitField('Sign Up')

//app.py
from forms import Form
@app.route('register')
def register():
    form = Form()
    return render_template('register.html', form=form)

//register.html
<form action="" method="POST">
    {{ form.hidden_tag() }}
    <fieldset class="form-group">
        <div class="form-group">
            {{ form.name.label(class='form-control-label') }}
            {{ form.name(class='form-control form-control-lg') }}
        </div>
    </fieldset>
    <div class="form-group">
        {{ form.submit(class='btn btn-outline-info') }}
    </div>
</form>


# Flask SQLAlchemy

from flask_sqlalchemy import SQLAlchemy
from datetime import datetime

app = Flask(__name__)
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///site.db'
db = SQLAlchemy(app)

//app.py
class Model1(db.Model):
    a = db.Column(db.Integer, primary_key=True)
    b = db.Column(db.String(100), nullable=False)
    c = db.Column(db.DateTime, nullable=False, default=datetime.utcnow)
    d = db.Column(db.Text, nullable=False)
    e = db.relationship('Model2', backref='anything', lazy=True)
    **(Model2)
    f = db.Column(db.Integer, db.ForeignKey('model1.id'), nullable=False)

    def __repr__(self):
        return f"Model1('{self.a}', '{self.b}')"

<!-- sql_query -->
from app import db
db.create_all()
user_1 = Model1(username='anish', email='anish@gmail.com', password='123')
db.session.add(user_1)
Model1.query.all()
Model1.query.first()
Model1.query.filter_by(username='anish').all()
Model1.query.get(id_value)
db.drop_all()
#Flask-learning
