---
nNPCs: 0
cssclasses:
  - fate-combat-tracker
sheetsFolder: Encounters/Combat 1
sheetsPrefix: NPC
sheetTemplate: "[[Template - NPC tracking]]"
sheetsPrevFolder: ""
sheetsPrevPrefix: ""
---

## `VIEW[{nNPCs}]` NPCs

```js-engine

const mb = engine.getPlugin('obsidian-meta-bind-plugin').api;
const comp = new obsidian.Component(component);

const property = "nNPCs";
const bindTarget = mb.parseBindTarget(property, context.file.path);
const bindTargetSheetsFolder = mb.parseBindTarget('sheetsFolder', context.file.path);
const bindTargetSheetsPrefix = mb.parseBindTarget('sheetsPrefix', context.file.path);

function createButton(property, type, id, n, sheetsFolder, sheetsPrefix)
{
	let label = "";
	let icon = "";
	let value = "";
	switch(type) {
		case "decrement":
			label = "-1";
			icon = "minus";
			value = "Math.max(0, x - 1)";
			n -= 1;
			break;
		case "reset":
			label = "Reset";
			icon = "rotate-ccw";
			value = "0";
			n = 0;
			break;
		case "increment":
			label = "+1";
			icon = "plus";
			value = "Math.min(10, x + 1)";
			n += 1;
			break;
	}
	
	actions = [];
	if (n > 0) {
		const fileName = `${sheetsPrefix} ${n.toString()}`;
		const filePath = `${sheetsFolder}/${fileName}.md`;
		actions.push({
			type: 'inlineJS',
			code: `console.log(app.vault.getFolderByPath('${sheetsFolder}')); !app.vault.getFolderByPath('${sheetsFolder}') ? await app.vault.createFolder('${sheetsFolder}') : '';`
		});
		actions.push({
			type: 'inlineJS',
			code: `
				const targetFile = app.vault.getFileByPath('${filePath}');
				if (targetFile) return;
				const current = app.workspace.getActiveFile();
				const metadata = app.metadataCache.getFileCache(current);
				let template = metadata.frontmatter['sheetTemplate'];
				if (template) {
					console.log(template);
					template = template.replaceAll('[', '');
					template = template.replaceAll(']', '');
					template = template + ".md";
					const templateFirstLinkpath = app.metadataCache.getFirstLinkpathDest(template, '');
					if (templateFirstLinkpath) {
						const templatePath = templateFirstLinkpath['path'];
						const templateFile = app.vault.getFileByPath(templatePath);
						await app.vault.copy(templateFile, '${filePath}');
						console.log("Copied");
						return;
					}
				}
				console.log("Creating ${filePath}");
				await app.vault.create('${filePath}', '');
				console.log("Created ${filePath}");
			`
		});
	}
	actions.push({
		type: 'updateMetadata',
		bindTarget: property,
		evaluate: true,
		value: value
	});
	
	const declaration = {
		label: label,
		icon: icon,
		style: "default",
		id: id,
		hidden: true,
		actions: actions
	};
	
	const buttonOptions = {
        declaration: declaration,
        isPreview: false
    };
    
    const button = mb.createButtonMountable(context.file.path, buttonOptions);
    return mb.wrapInMDRC(button, container, component);
}

function render(n, sheetsFolder, sheetsPrefix) {
	const mdBuilder = engine.markdown.createBuilder();
	comp.unload();
	comp.load();
	container.empty();
	
	let buttonIDLists = "";
	for (type of ["decrement", "reset", "increment"]) {
		let id = property + "-" + type;
		createButton(property, type, id, n, sheetsFolder, sheetsPrefix);
		buttonIDLists += id + ",";
	}
	buttonIDLists = buttonIDLists.slice(0, -1); 
	mdBuilder.createParagraph("`" + `BUTTON[${buttonIDLists}]` + "`");
	return mdBuilder;
}

return mb.reactiveMetadata([bindTarget, bindTargetSheetsFolder, bindTargetSheetsPrefix], component, (nNPCs, sheetsFolder, sheetsPrefix) => {
	while (sheetsFolder.charAt(sheetsFolder.length - 1) == "/") {
		sheetsFolder = sheetsFolder.slice(0, -1);
	}
	return render(nNPCs, sheetsFolder, sheetsPrefix);
});

```


```js-engine

const mb = engine.getPlugin('obsidian-meta-bind-plugin').api;
const comp = new obsidian.Component(component);

const bindTargetnNPCs = mb.parseBindTarget('nNPCs', context.file.path);
const bindTargetSheetsFolder = mb.parseBindTarget('sheetsFolder', context.file.path);
const bindTargetSheetsPrefix = mb.parseBindTarget('sheetsPrefix', context.file.path);

function render(nNPCs, sheetsFolder, sheetsPrefix) {
	const mdBuilder = engine.markdown.createBuilder();
	comp.unload();
	comp.load();
	container.empty();
	
	for (let n = 0; n < nNPCs; ++n) {
		let fileBaseName = `${sheetsPrefix} ${n+1}`;
		let note = `${sheetsFolder}/${fileBaseName}`;
		let name = "Name: `" + `INPUT[text:${note}#name]` + "`";
		let calloutEnemy = mdBuilder.createCallout(fileBaseName, "fate-npc", "");
		
		calloutEnemy.createParagraph(name);
		calloutEnemy.createParagraph("Take out `" + `INPUT[toggle(class(npc-out)):${note}#takenOut]` + "`");
		
		let calloutAspects = calloutEnemy.createCallout("Character aspects", "fate-aspects", "");
		let headers = ['', 'Name'];
		let body = [];
		/* High Concept */
		body.push(['High Concept', "`" + `INPUT[text:${note}#concept]` + "`"]);
		/* Trouble */
		body.push(['Trouble', "`" + `INPUT[text:${note}#trouble]` + "`"]);
		/* Character Aspects */
		for (let i = 0; i < 3; i++) {
			const label = `Aspect ${i+1}`;
			const input = "`" + `INPUT[text:${note}#aspect${i+1}]` + "`";
			body.push([label, input]);
		}
		calloutAspects.createTable(headers, body);
		
		let calloutHealth = calloutEnemy.createCallout("Stress and Consequences", "fate-health", "");
		headers = ['', 'Used'];
		body = [];
		/* Stress */
		for (let i = 0; i < 3; i++) {
			const label = `Stress ${i+1}`;
			const input = "`" + `INPUT[toggle:${note}#stress${i+1}]` + "`";
			body.push([label, input]);
		}
		/* Consequences */
		for (let i = 0; i < 3; i++) {
			const label = `Consequence ${i*2+2}`;
			const input = "`" + `INPUT[toggle:${note}#consequence${i*2+2}]` + "`";
			body.push([label, input]);
		}
		calloutHealth.createTable(headers, body);
		
		/* Other Aspects */
		let calloutOther = calloutEnemy.createCallout("Other aspects", "fate-other", "");
		const content = [`
			const mb = engine.getPlugin('obsidian-meta-bind-plugin').api;
			const bindTargetOtherAspects = mb.createBindTarget('frontmatter', '${note}.md', ['otherAspects']);
		
			const tableOtherAspectsOptions = {
				bindTarget: bindTargetOtherAspects,
				tableHead: ['Aspect', 'Type', 'Free Invocation', 'Value'],
				columns: [
					'INPUT[text:scope^name]',
					'INPUT[inlineSelect(option(Game), option(Character), option(Situation), option(Consequence), option(Boost)):scope^type]',
					'INPUT[number:scope^namefreeInvocation]',
					'INPUT[number(class(meta-bind-small-width)):scope^value]'
				],
			};
			
			const tableOtherAspectsMountable = mb.createTableMountable('${note}.md', tableOtherAspectsOptions);
			
			mb.wrapInMDRC(tableOtherAspectsMountable, container, component);
		`].join("\n");
		const mbCodeBlock = calloutOther.createCodeBlock("js-engine", content);
	}
	return mdBuilder;
}

return mb.reactiveMetadata([bindTargetnNPCs, bindTargetSheetsFolder, bindTargetSheetsPrefix], component, (nNPCs, sheetsFolder, sheetsPrefix) => {
	while (sheetsFolder.charAt(sheetsFolder.length - 1) == "/") {
		sheetsFolder = sheetsFolder.slice(0, -1);
	}
	return render(nNPCs, sheetsFolder, sheetsPrefix);
});
```


```js-engine
const mb = engine.getPlugin('obsidian-meta-bind-plugin').api;
const nNPCs     = mb.parseBindTarget('nNPCs', context.file.path);
const sheetsFolder = mb.parseBindTarget('sheetsFolder', context.file.path);
const sheetsPrevFolder = mb.parseBindTarget('sheetsPrevFolder', context.file.path);
const sheetsPrefix = mb.parseBindTarget('sheetsPrefix', context.file.path);
const sheetsPrevPrefix = mb.parseBindTarget('sheetsPrevPrefix', context.file.path);

return mb.reactiveMetadata([sheetsFolder, sheetsPrefix], component, (newFolder, newPrefix) => {
	let previousFolder = mb.getMetadata(sheetsPrevFolder);
	let previousPrefix = mb.getMetadata(sheetsPrevPrefix);
	while (newFolder.charAt(newFolder.length - 1) == "/") {
		newFolder = newFolder.slice(0, -1);
	}
	if (previousFolder) {
		while (previousFolder.charAt(previousFolder.length - 1) == "/") {
			previousFolder = previousFolder.slice(0, -1);
		}
	}
	if (previousFolder != newFolder || previousPrefix != newPrefix) {
		mb.setMetadata(sheetsPrevFolder, newFolder);
		mb.setMetadata(sheetsPrevPrefix, newPrefix);
		mb.setMetadata(nNPCs, 0);
	}
})
```