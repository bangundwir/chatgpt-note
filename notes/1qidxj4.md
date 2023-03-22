Model: GPT-4

> file ini ini berada di SystemPrompt.tsx import { Conversation } from "@/types"; import { DEFAULT\_SYSTEM\_PROMPT } from "@/utils/app/const"; import { FC, useEffect, useRef, useState } from "react"; interface Props { conversation: Conversation; onChangePrompt: (prompt: string) => void; } export const SystemPrompt: FC<Props> = ({ conversation, onChangePrompt }) => { const \[value, setValue\] = useState<string>(""); const textareaRef = useRef<HTMLTextAreaElement>(null); const handleChange = (e: React.ChangeEvent<HTMLTextAreaElement>) => { const value = e.target.value; const maxLength = 4000; if (value.length > maxLength) { alert(\`Prompt limit is ${maxLength} characters\`); return; } setValue(value); if (value.length > 0) { onChangePrompt(value); } }; useEffect(() => { if (textareaRef && textareaRef.current) { textareaRef.current.style.height = "inherit"; textareaRef.current.style.height = \`${textareaRef.current?.scrollHeight}px\`; } }, \[value\]); useEffect(() => { if (conversation.prompt) { setValue(conversation.prompt); } else { setValue(DEFAULT\_SYSTEM\_PROMPT); } }, \[conversation\]); return ( <div className="flex flex-col"> <label className="text-left dark:text-neutral-400 text-neutral-700 mb-2">System Prompt</label> <textarea ref={textareaRef} className="w-full rounded-lg px-4 py-2 focus:outline-none dark:bg-\[#40414F\] dark:border-opacity-50 dark:border-neutral-800 dark:text-neutral-100 border border-neutral-500 shadow text-neutral-900" style={{ resize: "none", bottom: \`${textareaRef?.current?.scrollHeight}px\`, maxHeight: "300px", overflow: "auto" }} placeholder="Enter a prompt" value={value} rows={1} onChange={handleChange} /> </div> ); }; Buatakan agar ada pilihan prompt sesui nama promt atau mode nanti di yang ada di const.tsx export const DEFAULT\_SYSTEM\_PROMPT = "You are ChatGPT, a large language model trained by OpenAI. Follow the user's instructions carefully. Respond using markdown.";

Untuk menambahkan pilihan prompt berdasarkan nama atau mode yang tersedia di `const.tsx`, Anda dapat melakukan langkah-langkah berikut:

1.  Tambahkan daftar pilihan prompt ke dalam `const.tsx`:

tsx

```tsx
export const PROMPT_OPTIONS = [
  {
    name: "ChatGPT",
    prompt: "You are ChatGPT, a large language model trained by OpenAI. Follow the user's instructions carefully. Respond using markdown."
  },
  {
    name: "Mode 2",
    prompt: "Ini adalah contoh prompt untuk mode 2."
  },
  // Tambahkan pilihan prompt lainnya di sini
];
```

2.  Import `PROMPT_OPTIONS` ke dalam `SystemPrompt.tsx`:

tsx

```tsx
import { Conversation } from "@/types";
import { DEFAULT_SYSTEM_PROMPT, PROMPT_OPTIONS } from "@/utils/app/const";
import { FC, useEffect, useRef, useState } from "react";
```

3.  Tambahkan sebuah fungsi untuk mengganti prompt berdasarkan pilihan:

tsx

```tsx
const handlePromptSelection = (selectedPrompt: string) => {
  const promptOption = PROMPT_OPTIONS.find(option => option.name === selectedPrompt);
  if (promptOption) {
    setValue(promptOption.prompt);
    onChangePrompt(promptOption.prompt);
  }
};
```

4.  Tambahkan elemen `<select>` untuk menampilkan daftar pilihan prompt:

tsx

```tsx
return (
  <div className="flex flex-col">
    <label className="text-left dark:text-neutral-400 text-neutral-700 mb-2">System Prompt</label>
    <select
      className="w-full mb-2 rounded-lg px-4 py-2 focus:outline-none dark:bg-[#40414F] dark:border-opacity-50 dark:border-neutral-800 dark:text-neutral-100 border border-neutral-500 shadow text-neutral-900"
      onChange={(e) => handlePromptSelection(e.target.value)}
    >
      {PROMPT_OPTIONS.map(option => (
        <option key={option.name} value={option.name}>
          {option.name}
        </option>
      ))}
    </select>
    <textarea
      // ...
    />
  </div>
);
```

Dengan perubahan ini, Anda akan memiliki elemen `<select>` yang menampilkan pilihan prompt yang tersedia. Ketika pengguna memilih prompt, teks di dalam elemen `<textarea>` akan diubah sesuai dengan pilihan yang dipilih.

> Buatakan fiture baru agar bisa melakukan Prompt Library buat file baru dengan mengunakan data dari ini https://raw.githubusercontent.com/f/awesome-chatgpt-prompts/main/prompts.csv dengan format "act","prompt" "Linux Terminal","I want you to act as a linux terminal. I will type commands and you will reply with what the terminal should show. I want you to only reply with the terminal output inside one unique code block, and nothing else. do not write explanations. do not type commands unless I instruct you to do so. when i need to tell you something in english, i will do so by putting text inside curly brackets {like this}. my first command is pwd"

Untuk membuat fitur Prompt Library, Anda dapat mengikuti langkah-langkah berikut:

1.  Buat file baru dengan nama `PromptLibrary.tsx` di direktori yang sesuai, misalnya di direktori `components`.
    
2.  Buat sebuah fungsi `fetchPrompts` untuk mengambil data dari URL yang diberikan:
    

tsx

```tsx
const fetchPrompts = async (): Promise<Array<{ act: string; prompt: string }>> => {
  const response = await fetch("https://raw.githubusercontent.com/f/awesome-chatgpt-prompts/main/prompts.csv");
  const csvText = await response.text();
  const lines = csvText.split("\n").slice(1); // Menghapus baris pertama (header)
  const prompts = lines.map(line => {
    const [act, prompt] = line.split(/,(.+)/); // Membedakan kolom "act" dan "prompt"
    return { act, prompt };
  });
  return prompts;
};
```

3.  Di dalam `PromptLibrary.tsx`, buat komponen `PromptLibrary`:

tsx

```tsx
import { FC, useEffect, useState } from "react";

interface Props {
  onSelectPrompt: (prompt: string) => void;
}

export const PromptLibrary: FC<Props> = ({ onSelectPrompt }) => {
  const [prompts, setPrompts] = useState<Array<{ act: string; prompt: string }>>([]);

  useEffect(() => {
    (async () => {
      const fetchedPrompts = await fetchPrompts();
      setPrompts(fetchedPrompts);
    })();
  }, []);

  const handlePromptSelection = (selectedPrompt: string) => {
    onSelectPrompt(selectedPrompt);
  };

  return (
    <div className="flex flex-col">
      <label className="text-left dark:text-neutral-400 text-neutral-700 mb-2">Prompt Library</label>
      <select
        className="w-full mb-2 rounded-lg px-4 py-2 focus:outline-none dark:bg-[#40414F] dark:border-opacity-50 dark:border-neutral-800 dark:text-neutral-100 border border-neutral-500 shadow text-neutral-900"
        onChange={(e) => handlePromptSelection(e.target.value)}
      >
        <option value="">Select a prompt</option>
        {prompts.map(({ act, prompt }, index) => (
          <option key={index} value={prompt}>
            {act}
          </option>
        ))}
      </select>
    </div>
  );
};
```

4.  Import komponen `PromptLibrary` ke dalam `SystemPrompt.tsx`:

tsx

```tsx
import { PromptLibrary } from "./PromptLibrary";
```

5.  Tambahkan komponen `PromptLibrary` di bawah elemen `<textarea>`:

tsx

```tsx
return (
  <div className="flex flex-col">
    <label className="text-left dark:text-neutral-400 text-neutral-700 mb-2">System Prompt</label>
    <select
      // ...
    </select>
    <textarea
      // ...
    />
    <PromptLibrary onSelectPrompt={onChangePrompt} />
  </div>
);
```

Sekarang Anda akan memiliki fitur Prompt Library yang menampilkan daftar prompt yang tersedia dari CSV yang diberikan. Ketika pengguna memilih prompt, teks di dalam elemen `<textarea>` akan diubah sesuai dengan pilihan yang dipilih.

> Mengalami erro Unhandled Runtime Error Error: Element type is invalid: expected a string (for built-in components) or a class/function (for composite components) but got: undefined. You likely forgot to export your component from the file it's defined in, or you might have mixed up default and named imports. Check the render method of \`SystemPrompt\`.

Untuk mengatasi kesalahan ini, pastikan bahwa Anda telah mengekspor komponen `PromptLibrary` dengan benar dari file `PromptLibrary.tsx` dan mengimpornya dengan benar di `SystemPrompt.tsx`.

Periksa langkah-langkah berikut:

1.  Pastikan Anda mengekspor komponen `PromptLibrary` sebagai _default export_ dari file `PromptLibrary.tsx`:

tsx

```tsx
export default PromptLibrary;
```

2.  Kemudian, ubah cara Anda mengimpor komponen `PromptLibrary` di file `SystemPrompt.tsx`. Jika Anda mengekspor `PromptLibrary` sebagai _default export_, Anda harus mengimpor tanpa menggunakan tanda kurung kurawal:

tsx

```tsx
import PromptLibrary from "./PromptLibrary";
```

Jika Anda telah mengikuti langkah-langkah ini dan masih mengalami kesalahan, pastikan bahwa Anda telah mengimpor dan mengekspor semua komponen dan dependensi lainnya dengan benar.

> Tulis lengkap PromptLibrary.tsx

Berikut ini adalah kode lengkap untuk file `PromptLibrary.tsx`:

tsx

```tsx
import React, { FC, useEffect, useState } from "react";

interface Props {
  onSelectPrompt: (prompt: string) => void;
}

const fetchPrompts = async (): Promise<Array<{ act: string; prompt: string }>> => {
  const response = await fetch("https://raw.githubusercontent.com/f/awesome-chatgpt-prompts/main/prompts.csv");
  const csvText = await response.text();
  const lines = csvText.split("\n").slice(1); // Menghapus baris pertama (header)
  const prompts = lines.map(line => {
    const [act, prompt] = line.split(/,(.+)/); // Membedakan kolom "act" dan "prompt"
    return { act, prompt };
  });
  return prompts;
};

const PromptLibrary: FC<Props> = ({ onSelectPrompt }) => {
  const [prompts, setPrompts] = useState<Array<{ act: string; prompt: string }>>([]);

  useEffect(() => {
    (async () => {
      const fetchedPrompts = await fetchPrompts();
      setPrompts(fetchedPrompts);
    })();
  }, []);

  const handlePromptSelection = (selectedPrompt: string) => {
    onSelectPrompt(selectedPrompt);
  };

  return (
    <div className="flex flex-col">
      <label className="text-left dark:text-neutral-400 text-neutral-700 mb-2">Prompt Library</label>
      <select
        className="w-full mb-2 rounded-lg px-4 py-2 focus:outline-none dark:bg-[#40414F] dark:border-opacity-50 dark:border-neutral-800 dark:text-neutral-100 border border-neutral-500 shadow text-neutral-900"
        onChange={(e) => handlePromptSelection(e.target.value)}
      >
        <option value="">Select a prompt</option>
        {prompts.map(({ act, prompt }, index) => (
          <option key={index} value={prompt}>
            {act}
          </option>
        ))}
      </select>
    </div>
  );
};

export default PromptLibrary;
```

Pastikan Anda mengekspor komponen `PromptLibrary` sebagai _default export_ dan mengimpornya dengan benar di file `SystemPrompt.tsx`. Jangan lupa untuk menggabungkan kode ini dengan kode `SystemPrompt.tsx` yang telah Anda modifikasi sebelumnya.

> buatakan agar ui nya lebih responsif dan bisa informatif

Untuk membuat UI lebih responsif dan informatif, Anda dapat menggabungkan Bootstrap atau Tailwind CSS untuk mengatur tata letak dan gaya, serta menambahkan elemen yang dapat membantu pengguna memahami cara kerja aplikasi. Berikut adalah beberapa contoh peningkatan yang dapat Anda lakukan:

1.  Tambahkan judul dan deskripsi singkat tentang aplikasi di bagian atas halaman.
    
2.  Kelompokkan elemen yang berkaitan untuk membantu