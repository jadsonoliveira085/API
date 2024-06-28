# API
Uma API com python e flask


from flask import Flask, jsonify, request

app = Flask(__name__)

Livros = [
    {
        'id': 1,
        'titulo': 'O Senhor dos Anéis - A Sociedade do Anel',
        'autor': 'J.R.R. Tolkien'
    },
    {
        'id': 2,
        'titulo': 'Harry Potter - A Pedra Filosofal',
        'autor': 'J.K. Rowling'
    },
    {
        'id': 3,
        'titulo': 'Hábitos Atômicos',
        'autor': 'James Clear'
    }
]

@app.route('/livros', methods=['GET']) 
def obter_livros():
    return jsonify(Livros)

@app.route('/livros/<int:id>', methods=['GET']) 
def obter_livro_id(id):
    for livro in Livros:
        if livro['id'] == id:  
            return jsonify(livro)
    return jsonify({'erro': 'Livro não encontrado'}), 404  

@app.route('/livros/<int:id>', methods=['PUT'])  
def editar_livro_por_id(id):
    livro_alterado = request.get_json()  
    for indice, livro in enumerate(Livros): 
        if livro['id'] == id:
            Livros[indice].update(livro_alterado)  
            return jsonify(Livros[indice])
    return jsonify({'erro': 'Livro não encontrado'}), 404


@app.route('/livros', methods=['POST'])
def incluir_novo_livro():
    novo_livro = request.get_json()
    novo_livro['id'] = Livros[-1]['id'] + 1 if Livros else 1
    Livros.append(novo_livro)
    return jsonify(novo_livro), 201

@app.route('/livros/<int:id>', methods=['DELETE'])
def excluir_livro(id):
    for indice, livro in enumerate(Livros):
        if livro.get('id') == id:
            del Livros[indice]
            return jsonify({'mensagem': 'Livro excluído com sucesso'})
    return jsonify({'erro': 'Livro não encontrado'}), 404

if __name__ == '__main__':
    app.run(port=5000, debug=True, host='0.0.0.0')
