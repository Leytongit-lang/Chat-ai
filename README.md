import React, { useState } from "react"; import { Card, CardContent } from "@/components/ui/card"; import { Input } from "@/components/ui/input"; import { Button } from "@/components/ui/button"; import { Select, SelectTrigger, SelectValue, SelectContent, SelectItem } from "@/components/ui/select"; import { ImageIcon } from "lucide-react";

const characters = { "Elsa (Frozen)": { personality: "calm, regal, nurturing", style: (msg) => *With a graceful tone* I understand, dear one. ${msg}, imagePrompt: "Elsa from Frozen smiling gracefully, snowy background, Disney style" }, "Hinata (Naruto)": { personality: "shy, kind, loyal", style: (msg) => *Blushes softly* Um... I think... ${msg}, imagePrompt: "Hinata from Naruto, shy smile, soft colors, anime style" }, // ... other characters ... "Cosmo (Fairly OddParents)": { personality: "goofy, silly, enthusiastic", style: (msg) => *Laughing* Yay! That reminds me of a banana! ${msg}, imagePrompt: "Cosmo from Fairly OddParents, goofy expression, floating with wand" }, "Wanda (Fairly OddParents)": { personality: "smart, sassy, loving", style: (msg) => *Sighs lovingly* Oh, Cosmo... ${msg}, imagePrompt: "Wanda from Fairly OddParents, floating and smiling, cartoon style" }, "Timmy Turner (Fairly OddParents)": { personality: "energetic, impulsive, kind-hearted", style: (msg) => *With excitement* I wish for... uh, wait! ${msg}, imagePrompt: "Timmy Turner wearing a pink cap, excited, Fairly OddParents style" } };

const ChatBotApp = () => { const [messages, setMessages] = useState([]); const [input, setInput] = useState(""); const [selectedCharacter, setSelectedCharacter] = useState("Elsa (Frozen)");

const sendMessage = () => { if (!input.trim()) return; const userMessage = { sender: "user", text: input }; setMessages((prev) => [...prev, userMessage]);

const char = characters[selectedCharacter];
let replyText = char.style("That's very interesting. Let's talk more about it.");

if (input.toLowerCase().includes("show me") || input.toLowerCase().includes("picture")) {
  replyText += ` [Image Prompt: ${char.imagePrompt}]`;
}

const botMessage = { sender: "bot", text: replyText };

setTimeout(() => {
  setMessages((prev) => [...prev, botMessage]);
}, 500);

setInput("");

};

return ( <div className="max-w-xl mx-auto p-4"> <h1 className="text-2xl font-bold mb-4 text-center">Chat with a Character</h1> <div className="mb-4"> <Select value={selectedCharacter} onValueChange={setSelectedCharacter}> <SelectTrigger> <SelectValue placeholder="Choose a character" /> </SelectTrigger> <SelectContent> {Object.keys(characters).map((name) => ( <SelectItem key={name} value={name}>{name}</SelectItem> ))} </SelectContent> </Select> </div> <Card className="h-[400px] overflow-y-auto p-4 mb-4"> <CardContent> {messages.map((msg, index) => ( <div key={index} className={mb-2 ${msg.sender === "user" ? "text-right" : "text-left"}}> <span className="block p-2 rounded bg-gray-200 dark:bg-gray-700 inline-block"> {msg.text.includes("[Image Prompt") ? ( <> {msg.text.split("[Image Prompt")[0]} <div className="mt-2"> <ImageIcon className="inline mr-2" /> Image generated: <em>{msg.text.split("[")[1].replace("]", "")}</em> </div> </> ) : msg.text} </span> </div> ))} </CardContent> </Card> <div className="flex gap-2"> <Input placeholder="Type your message..." value={input} onChange={(e) => setInput(e.target.value)} onKeyDown={(e) => e.key === "Enter" && sendMessage()} /> <Button onClick={sendMessage}>Send</Button> </div> </div> ); };

export default ChatBotApp;

