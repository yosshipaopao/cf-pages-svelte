<script lang="ts">
	import { signIn } from '@auth/sveltekit/client';
	import type { PageData, ActionData } from './$types';
	import { enhance } from '$app/forms';

	export let data: PageData;
	export let form: ActionData;

	$: {
		// call here on change
		console.log(data.session?.user);
	}
</script>

<form
	method="post"
	class="mx-auto flex w-3/4 flex-col gap-8 py-20"
	use:enhance
	enctype="multipart/form-data"
>
	<div>
		{JSON.stringify(data.session?.user)}
	</div>

	{#if form?.success}
		<p class="text-3xl font-bold text-accent">Success!!!!</p>
	{/if}

	{#if form?.missing}
		<p class="text-3xl font-bold text-accent">Missing Form</p>
		/
	{/if}

	<div class="form-control">
		<button class="btn btn-primary" on:click={() => signIn()}> Sign In with Discord </button>
	</div>

	<input type="hidden" name="username" value={data.session?.user?.name} />

	<div class="form-control">
		<span class="label-text text-xl">1.</span>
		<label class="label cursor-pointer justify-start gap-4">
			<input name="agree" value="ok" required type="checkbox" class="checkbox-primary checkbox" />
			<span class="label-text"> Discordで必須条件、注意事項を読みました。 </span>
		</label>
	</div>

	<div class="form-control gap-2">
		<span class="text-xl">2.</span>
		<span class="label-text"> メイン作業(18:00~26:00)に何時頃から参加できるか </span>
		<span class="p-10">
			<select name="work-start" required class="select select-primary w-full max-w-xs">
				<option disabled selected> 回答を選択 </option>
				{#each [['18', '18:00~'], ['19', '19:00~'], ['20', '20:00~'], ['21', '21:00~'], ['22', '22:00~']] as v}
					<option value={v[0]} selected={form?.work_start === v[0]}>{v[1]}</option>
				{/each}
			</select>
		</span>
	</div>

	<div class="form-control gap-2">
		<span class="text-xl">3.</span>
		<span class="label-text"> 2で記述した時間帯から具体的に何時間来れるか </span>

		<span class="p-10">
			<input
				name="work-time"
				type="range"
				min="0"
				max="8"
				step="1"
				value={form?.work_time ?? 0}
				class="range range-primary"
			/>
			<span class="flex w-full justify-between px-2 text-xs">
				{#each [0, 1, 2, 3, 4, 5, 6, 7, 8] as i}
					<span>{i}</span>
				{/each}
			</span>
		</span>
	</div>

	<div class="form-control">
		<span class="text-xl">4.</span>
		<span class="label-text">得意な分野</span>

		<span class="p-10">
			<label class="label cursor-pointer">
				<span class="label-text">作業</span>
				<input name="skill" value="work" type="checkbox" class="checkbox-primary checkbox" />
			</label>
			<label class="label cursor-pointer">
				<span class="label-text">建築</span>
				<input
					name="skill"
					value="architecture"
					type="checkbox"
					class="checkbox-primary checkbox"
				/>
			</label>
			<label class="label cursor-pointer">
				<span class="label-text">回路</span>
				<input name="skill" value="develop" type="checkbox" class="checkbox-primary checkbox" />
			</label>
		</span>
	</div>

	<label class="form-control">
		<span class="label-text text-xl">5.</span>
		<span class="label-text">
			Tweakeroo・Litematica・Item scroller・MiniHUDそれぞれについて知っている機能を記述
		</span>
		<span class="p-10">
			<textarea
				name="knowledge"
				required
				value={form?.knowledge ?? ''}
				class="textarea textarea-primary textarea-lg w-full"
				placeholder="回答を入力"
			></textarea>
		</span>
	</label>

	<label class="form-control">
		<span class="label-text text-xl">6.</span>
		<span class="label-text">PCスペック</span>
		<span class="p-10">
			<textarea
				name="pc-spec"
				required
				value={form?.pc_spec ?? ''}
				class="textarea textarea-primary textarea-lg w-full"
				placeholder="回答を入力"
			></textarea>
		</span>
	</label>

	<label class="form-control">
		<span class="label-text text-xl">6.</span>
		<span class="label-text">
			これまでにした作業、作った装置、建築の画像または設計図 1つのファイル最大10MB、合計5つまで
		</span>
		<span class="p-10">
			<input
				name="files"
				type="file"
				required
				accept="image/*"
				multiple
				class="file-input file-input-bordered file-input-primary w-full"
			/>
		</span>
	</label>

	<label class="form-control">
		<span class="label-text text-xl">7.</span>
		<span class="label-text">自己PR</span>
		<span class="p-10">
			<textarea
				name="pr"
				required
				value={form?.pr ?? ''}
				class="textarea textarea-primary textarea-lg w-full"
				placeholder="回答を入力"
			></textarea>
		</span>
	</label>

	<label class="form-control">
		<span class="label-text text-xl">8.</span>
		<span class="label-text">そのほか伝えたいこと</span>
		<span class="p-10">
			<textarea
				name="other"
				value={form?.other ?? ''}
				class="textarea textarea-primary textarea-lg w-full"
				placeholder="回答を入力"
			></textarea>
		</span>
	</label>

	<div class="form-control">
		<input type="submit" class="btn btn-outline btn-primary btn-wide" value="送信" />
	</div>
</form>
