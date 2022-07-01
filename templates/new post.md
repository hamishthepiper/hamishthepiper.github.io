<%*
<!-- https://github.com/SilentVoid13/Templater/discussions/569 -->
async function createFolder(dir, times = 1) {
        if (times > 5) {
            throw new Error('createFolder: too many tries');
        }
        try {
            let newDir = dir;
            if (times > 1) newDir = `${dir} (${times})`;
            await this.app.vault.createFolder(newDir);
            return newDir;
        } catch (err) {
            return createFolder(dir, times + 1);
        }
    }
  
  let blog_name = await tp.system.prompt("Blog Title");
  let dir = `/content/post/${blog_name}`;
  dir = await createFolder(dir);
  await tp.file.move(`${dir}/index`);
-%>
---
title: <% blog_name %>
date: <% tp.date.now("YYYY-MM-DD") %>
categories: 
- <% tp.system.suggester(["Admin", "DFIR", "General IT"], ["Admin", "DFIR", "General IT"]) %>
tags: 
- 
---
## TLDR
<% tp.file.cursor(1) %>
