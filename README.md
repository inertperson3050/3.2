# 3.2
Library Management Ui 


import React, { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { motion } from "framer-motion";
import { Trash2, Plus, Search } from "lucide-react";

export default function LibraryApp() {
  const [books, setBooks] = useState([
    { id: 1, title: "The Great Gatsby", author: "F. Scott Fitzgerald" },
    { id: 2, title: "1984", author: "George Orwell" },
    { id: 3, title: "To Kill a Mockingbird", author: "Harper Lee" },
  ]);

  const [search, setSearch] = useState("");
  const [newBook, setNewBook] = useState({ title: "", author: "" });

  const handleAddBook = () => {
    if (newBook.title && newBook.author) {
      setBooks([...books, { id: Date.now(), ...newBook }]);
      setNewBook({ title: "", author: "" });
    }
  };

  const handleRemoveBook = (id) => {
    setBooks(books.filter((book) => book.id !== id));
  };

  const filteredBooks = books.filter(
    (book) =>
      book.title.toLowerCase().includes(search.toLowerCase()) ||
      book.author.toLowerCase().includes(search.toLowerCase())
  );

  return (
    <div className="min-h-screen bg-gradient-to-br from-purple-500 via-pink-400 to-blue-400 p-6">
      <h1 className="text-4xl font-extrabold mb-6 text-center text-white drop-shadow-md">
        📚 Library Management
      </h1>

      {/* Search Bar */}
      <div className="flex gap-2 mb-6 max-w-xl mx-auto">
        <Input
          placeholder="Search by title or author..."
          value={search}
          onChange={(e) => setSearch(e.target.value)}
          className="bg-white/80"
        />
        <Button variant="outline" className="bg-white/80">
          <Search className="w-4 h-4" />
        </Button>
      </div>

      {/* Add Book Section */}
      <div className="flex gap-2 mb-6 max-w-xl mx-auto">
        <Input
          placeholder="Book Title"
          value={newBook.title}
          onChange={(e) => setNewBook({ ...newBook, title: e.target.value })}
          className="bg-white/80"
        />
        <Input
          placeholder="Author"
          value={newBook.author}
          onChange={(e) => setNewBook({ ...newBook, author: e.target.value })}
          className="bg-white/80"
        />
        <Button onClick={handleAddBook} className="bg-gradient-to-r from-green-400 to-green-600 text-white">
          <Plus className="w-4 h-4" /> Add
        </Button>
      </div>

      {/* Book List */}
      <div className="grid gap-4 max-w-2xl mx-auto">
        {filteredBooks.length > 0 ? (
          filteredBooks.map((book) => (
            <motion.div
              key={book.id}
              initial={{ opacity: 0, y: 20 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.3 }}
            >
              <Card className="shadow-md bg-white/90">
                <CardContent className="flex justify-between items-center p-4">
                  <div>
                    <h2 className="text-lg font-semibold text-gray-800">{book.title}</h2>
                    <p className="text-gray-600">{book.author}</p>
                  </div>
                  <Button
                    variant="destructive"
                    onClick={() => handleRemoveBook(book.id)}
                    className="bg-gradient-to-r from-red-500 to-red-700 text-white"
                  >
                    <Trash2 className="w-4 h-4" />
                  </Button>
                </CardContent>
              </Card>
            </motion.div>
          ))
        ) : (
          <p className="text-center text-white/90">No books found.</p>
        )}
      </div>
    </div>
  );
}
