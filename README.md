# javascript-code-for-tiptap
npx create-react-app tiptap-extension-demo
cd tiptap-extension-demo
npm install @tiptap/core @tiptap/react @tiptap/extension-text @tiptap/extension-starter-kit
///EmojiExtension.js:
import { Node } from '@tiptap/core';

const EmojiExtension = Node.create({
  name: 'emoji',

  group: 'inline',

  inline: true,

  atom: true,

  addAttributes() {
    return {
      emoji: {
        default: '😊',
      },
    };
  },

  parseHTML() {
    return [
      {
        tag: 'span[data-emoji]',
      },
    ];
  },

  renderHTML({ HTMLAttributes }) {
    return ['span', { 'data-emoji': '', ...HTMLAttributes }, HTMLAttributes.emoji];
  },

  addCommands() {
    return {
      setEmoji: (emoji) => ({ commands }) => {
        return commands.insertContent({
          type: this.name,
          attrs: { emoji },
        });
      },
    };
  },
});

export default EmojiExtension;
///App.js:
import React from 'react';
import { EditorContent, useEditor } from '@tiptap/react';
import { StarterKit } from '@tiptap/starter-kit';
import EmojiExtension from './EmojiExtension';

const App = () => {
  const editor = useEditor({
    extensions: [
      StarterKit,
      EmojiExtension,
    ],
    content: '',
  });

  const addEmoji = () => {
    editor.chain().focus().setEmoji('😊').run();
  };

  return (
    <div>
      <button onClick={addEmoji}>Add Emoji</button>
      <EditorContent editor={editor} />
    </div>
  );
};

export default App;
