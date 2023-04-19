<template>
	<main class="content">
		<section class="desk">
			<div class="desk__header">
				<h1 class="desk__title">Design Coffee Lab</h1>
				<div class="desk__filters">
					<div class="desk__user-filter">
						<ul class="user-filter">
							<li class="user-filter__item" v-for="user in users" :key="user.id" :title="user.name">
								<a class="user-filter__button">
									<img :src="getImage(user.avatar)" alt="Аватар юзера" width="24" height="24" />
								</a>
							</li>
						</ul>
					</div>
					<div class="desk__meta-filter">
						<ul class="meta-filter">
							<li class="meta-filter__item" v-for="{ value, label } in STATUSES" :key="value">
								<a
									href=""
									class="meta-filter__status"
									:class="`meta-filter__status meta-filter__status--color meta-filter__status--${value}`"
									:title="label"
								/>
							</li>
						</ul>
					</div>
				</div>
			</div>

			<div class="desk__columns" v-if="columns.length">
				<div class="column" v-for="column in columns" :key="column.id">
					<h2 class="column__name">Название колонки</h2>
					<div class="column__target-area">
						<div class="column__task" v-for="task in columnTasks[column.id]">
							<div class="task">
								<div class="task__user" v-if="task.user">
									<div class="task__avatar">
										<img :src="getImage(task.user.avatar)" :alt="task.user.name" width="20" height="20" />
									</div>
									{{ task.user.name }}
								</div>

								<div class="task__statuses">
									<span class="task__status" v-if="task.status" :class="`task__status--${task.status}`" />
									<span class="task__status" v-if="task.timeStatus" :class="`task__status--${task.timeStatus}`" />
								</div>

								<h5 class="task__title" :class="{ 'task__title--first': !task.user }">{{ task.title }}</h5>
								<ul class="task__tags" v-if="task.tags && task.tags.length">
									<li v-for="(tag, index) in task.tags" :key="index">
										<span class="tag tag--blue"> {{ tag }} </span>
									</li>
								</ul>
							</div>
						</div>
					</div>
				</div>
			</div>

			<p class="desk__emptiness" v-else>Пока нет ни одной колонки</p>
		</section>
	</main>
</template>

<script setup>
import columns from '../mocks/columns.json';
import users from '../mocks/users.json';
import { STATUSES } from '../common/constants';
import rawTasks from '../mocks/tasks.json';
import { normalizeTask, getTagsArrayFromString } from '../common/helpers';

const getImage = (image) => {
	// https://vitejs.dev/guide/assets.html#new-url-url-import-meta-url
	return new URL(`../assets/img/${image}`, import.meta.url).href;
};

const normalizeTasks = rawTasks.map((task) => normalizeTask(task));

const columnTasks = normalizeTasks
	.filter(({ columnId }) => columnId)
	.reduce((acc, task) => {
		task.tags = getTagsArrayFromString(task.tags);

		if (acc[task.columnId]) {
			acc[task.columnId] = [...acc[task.columnId], task];
		} else {
			acc[task.columnId] = [task];
		}

		return acc;
	});
</script>
