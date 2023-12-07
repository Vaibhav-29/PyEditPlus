# PyEditPlus
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QTextEdit, QAction, QFileDialog


class TextEditor(QMainWindow):
    def __init__(self):
        super().__init__()

        self.init_ui()

    def init_ui(self):
        # Create a QTextEdit widget for text editing
        self.text_edit = QTextEdit(self)
        self.setCentralWidget(self.text_edit)

        # Create actions for menu items
        new_action = QAction('New', self)
        new_action.triggered.connect(self.new_file)

        open_action = QAction('Open', self)
        open_action.triggered.connect(self.open_file)

        save_action = QAction('Save', self)
        save_action.triggered.connect(self.save_file)

        cut_action = QAction('Cut', self)
        cut_action.triggered.connect(self.text_edit.cut)

        copy_action = QAction('Copy', self)
        copy_action.triggered.connect(self.text_edit.copy)

        paste_action = QAction('Paste', self)
        paste_action.triggered.connect(self.text_edit.paste)

        undo_action = QAction('Undo', self)
        undo_action.triggered.connect(self.text_edit.undo)

        redo_action = QAction('Redo', self)
        redo_action.triggered.connect(self.text_edit.redo)

        # Create menu bar and add actions
        menubar = self.menuBar()
        file_menu = menubar.addMenu('File')
        file_menu.addAction(new_action)
        file_menu.addAction(open_action)
        file_menu.addAction(save_action)

        edit_menu = menubar.addMenu('Edit')
        edit_menu.addAction(cut_action)
        edit_menu.addAction(copy_action)
        edit_menu.addAction(paste_action)
        edit_menu.addSeparator()
        edit_menu.addAction(undo_action)
        edit_menu.addAction(redo_action)

        # Set window properties
        self.setGeometry(100, 100, 800, 600)
        self.setWindowTitle('Text Editor')
        self.show()

    def new_file(self):
        self.text_edit.clear()

    def open_file(self):
        options = QFileDialog.Options()
        file_name, _ = QFileDialog.getOpenFileName(self, "Open File", "", "Text Files (*.txt);;All Files (*)", options=options)
        if file_name:
            with open(file_name, 'r') as file:
                content = file.read()
                self.text_edit.setPlainText(content)

    def save_file(self):
        options = QFileDialog.Options()
        file_name, _ = QFileDialog.getSaveFileName(self, "Save File", "", "Text Files (*.txt);;All Files (*)", options=options)
        if file_name:
            with open(file_name, 'w') as file:
                file.write(self.text_edit.toPlainText())


if __name__ == '__main__':
    app = QApplication(sys.argv)
    editor = TextEditor()
    sys.exit(app.exec_())
