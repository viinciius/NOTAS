import sqlite3
from tkinter import *
from tkinter import ttk, messagebox
import re


def conectar_banco():
    conexao = sqlite3.connect("sistema_notas.db")
    cursor = conexao.cursor()
    cursor.execute("""
        CREATE TABLE IF NOT EXISTS alunos (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            nome TEXT NOT NULL,
            nota1 REAL NOT NULL,
            nota2 REAL NOT NULL
        )
    """)
    conexao.commit()
    conexao.close()

def inserir_aluno():
    nome = entry_nome.get()
    
    if not re.match("^[A-Za-zÀ-ÿ ]+$", nome):
        messagebox.showerror("Erro", "O nome deve conter apenas letras e espaços.")
        return
    
    try:
        nota1 = float(entry_nota1.get())
        nota2 = float(entry_nota2.get())
    except ValueError:
        messagebox.showerror("Erro", "As notas devem conter apenas números e ponto decimal, sem letras.")
        return

    conexao = sqlite3.connect("sistema_notas.db")
    cursor = conexao.cursor()
    cursor.execute("INSERT INTO alunos (nome, nota1, nota2) VALUES (?, ?, ?)", (nome, nota1, nota2))
    conexao.commit()
    conexao.close()
    carregar_dados()
    entry_nome.delete(0, END)
    entry_nota1.delete(0, END)
    entry_nota2.delete(0, END)
    messagebox.showinfo("Sucesso", "Aluno adicionado com sucesso!")

def carregar_dados():
    for item in my_tree.get_children():
        my_tree.delete(item)
    
    conexao = sqlite3.connect("sistema_notas.db")
    cursor = conexao.cursor()
    cursor.execute("SELECT id, nome, nota1, nota2 FROM alunos")
    rows = cursor.fetchall()
    for row in rows:
        media = (row[2] + row[3]) / 2
        situacao = "Aprovado" if media >= 6 else "Reprovado"
        my_tree.insert("", "end", values=(row[1], row[2], row[3], f"{media:.2f}", situacao))
    conexao.close()

root = Tk()
root.title("Sistema de Notas")

Label(root, text="Nome do Aluno:").grid(row=0, column=0, padx=10, pady=10)
entry_nome = Entry(root)
entry_nome.grid(row=0, column=1, padx=10, pady=10)

Label(root, text="Nota 1:").grid(row=1, column=0, padx=10, pady=10)
entry_nota1 = Entry(root)
entry_nota1.grid(row=1, column=1, padx=10, pady=10)

Label(root, text="Nota 2:").grid(row=2, column=0, padx=10, pady=10)
entry_nota2 = Entry(root)
entry_nota2.grid(row=2, column=1, padx=10, pady=10)

# Botão para adicionar aluno
btn_add = Button(root, text="Adicionar Aluno", command=inserir_aluno)
btn_add.grid(row=3, column=0, columnspan=2, pady=10)

# Treeview para mostrar os alunos e situação
my_tree = ttk.Treeview(root, columns=("Nome", "Nota 1", "Nota 2", "Média", "Situação"), show="headings")
my_tree.heading("Nome", text="Nome")
my_tree.heading("Nota 1", text="Nota 1")
my_tree.heading("Nota 2", text="Nota 2")
my_tree.heading("Média", text="Média")
my_tree.heading("Situação", text="Situação")
my_tree.grid(row=4, column=0, columnspan=2, padx=10, pady=10)

# Conecta ao banco e carrega os dados na inicialização
conectar_banco()
carregar_dados()

root.mainloop()
