erDiagram
    USER ||--o{ DOCUMENT : "Автор"
    USER {
        int id PK
        string name
        string role
    }
    DOCUMENT {
        int id PK
        string title
        string content
        string status
        int author_id FK
    }
    DOCUMENT ||--o{ APPROVAL : "Согласование"
    APPROVAL {
        int id PK
        int document_id FK
        int approver_id FK
        string status
    } 




class Document:
    def __init__(self, id, title, content, author_id, status="Draft"):
        self.id = id
        self.title = title
        self.content = content
        self.author_id = author_id
        self.status = status

class User:
    def __init__(self, id, name, role):
        self.id = id
        self.name = name
        self.role = role

class EDOSystem:
    def __init__(self):
        self.documents = []
        self.users = []

    def add_document(self, title, content, author_id):
        doc_id = len(self.documents) + 1
        self.documents.append(Document(doc_id, title, content, author_id))
        return doc_id

    def list_documents(self):
        return [(doc.id, doc.title, doc.status) for doc in self.documents]

    def update_status(self, doc_id, new_status):
        for doc in self.documents:
            if doc.id == doc_id:
                doc.status = new_status
                return True
        return False

# Пример использования
if __name__ == "__main__":
    edo = EDOSystem()
    user = User(1, "Иван Петров", "Author")
    doc_id = edo.add_document("Договор", "Текст договора...", user.id)
    print(edo.list_documents())
    edo.update_status(doc_id, "На согласовании")
    print(edo.list_documents())
