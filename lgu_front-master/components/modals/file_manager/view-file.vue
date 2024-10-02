<script setup lang = "ts">   
    import {
      DialogContent,
      DialogDescription,
      DialogFooter,
      DialogHeader,
      DialogTitle,
    } from '@/components/ui/dialog'
    const handleSave = () => {  

    }
</script>
<template>
    <DialogContent class="sm:max-w-[425px]">
      <DialogHeader>
        <DialogTitle>View File</DialogTitle>
        <DialogDescription>
          Please fill up the required form
        </DialogDescription>
      </DialogHeader>
      <div class="grid gap-4 py-4">
        <div class="grid grid-cols-4 items-center gap-4">
          <Label for="name" class="text-right">
            Name
          </Label>
          <Input id="name" value="Pedro Duarte" class="col-span-3" />
        </div>
        <div class="grid grid-cols-4 items-center gap-4">
          <Label for="username" class="text-right">
            File
          </Label>
          <Input id="username" type="file" class="col-span-3" />
        </div>
      </div>
      <DialogFooter>
        <Button type="submit">
          Save changes
        </Button>
      </DialogFooter>
    </DialogContent>
  </template>