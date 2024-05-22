---
layout: default
title: About
permalink: /about/
---

<style>
  .terminal {
    font-family: monospace;
    background-color: #000;
    color: #0f0;
    padding: 10px;
    border-radius: 5px;
    overflow: hidden;
    position: relative;
  }

  .placeholder {
    color: #0f0;
    opacity: 0.5;
    position: absolute;
    top: 10px;
    left: 10px;
  }

  .prompt {
    color: #0f0;
  }

  .output {
    color: #0f0;
    display: inline-block;
    overflow: hidden;
    white-space: pre-wrap; /* Ensures white spaces and newlines are preserved */
  }

  .input-line {
    display: flex;
  }

  .input-line input {
    background: none;
    border: none;
    color: #0f0;
    outline: none;
    width: 100%;
    font-family: monospace;
  }
</style>

<div class="terminal">
  <div class="placeholder">Please enter a command from the following: whoami, about, spell, help</div>
  <div class="output" id="terminal-output"></div>
  <div class="input-line">
    <span class="prompt">$</span>
    <input type="text" id="terminal-input" autofocus>
  </div>
</div>

<script>
  const outputElement = document.getElementById('terminal-output');
  const inputElement = document.getElementById('terminal-input');
  const placeholderElement = document.querySelector('.placeholder');

  const commands = {
    whoami: "Tom Abai, \nI'm a Security Researcher. \nSpecialize in: \n* Malware Analysis \n* supply chain attacks \n* vulnerability management",
    about: "This blog, Infestum Dissecto, \nfocuses on malware analysis \nand supply chain attacks with a touch of magic. \nDive deep into the world of cybersecurity \nwith unique insights and detailed dissections.",
    spell: function() {
      const spells = [
        "Expelliarmus - Disarms your malware defenses",
        "Expecto Patronum - Protects against phishing attacks",
        "Alohomora - Unlocks encrypted files",
        "Lumos - Sheds light on hidden processes",
        "Protego - Shields from zero-day exploits",
        "Obliviate - Erases traces of malware",
        "Sectumsempra - Severely damages malware structures",
        "Incendio - Burns malicious scripts",
        "Reparo - Fixes corrupted files",
        "Confundo - Confuses malware behavior"
      ];
      return spells[Math.floor(Math.random() * spells.length)];
    },
    help: "Available commands: whoami, about, spell, help"
  };

  function handleCommand(command) {
    switch (command.trim()) {
      case 'whoami':
      case 'about':
      case 'help':
        return commands[command];
      case 'spell':
        return commands.spell();
      default:
        return "Unrecognized command: " + command;
    }
  }

  function typeText(text, outputElement, callback) {
    let index = 0;
    function type() {
      if (index < text.length) {
        outputElement.innerHTML += text[index] === '\n' ? '<br>' : text[index];
        index++;
        setTimeout(type, 50); // Typing speed
      } else if (callback) {
        callback();
      }
    }
    type();
  }

  inputElement.addEventListener('keydown', function(event) {
    if (event.key === 'Enter') {
      const command = inputElement.value.trim();
      outputElement.innerHTML += `<div><span class="prompt">$</span> ${command}</div>`;
      const result = handleCommand(command);
      const resultElement = document.createElement('div');
      outputElement.appendChild(resultElement);
      placeholderElement.style.display = 'none';
      typeText(result, resultElement, function() {
        inputElement.value = '';
        outputElement.scrollTop = outputElement.scrollHeight; // Scroll to the bottom
      });
    }
  });

  inputElement.addEventListener('input', function() {
    placeholderElement.style.display = inputElement.value ? 'none' : 'block';
  });
</script>
